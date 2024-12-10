# DRAFT \*\*\*\*

### Summary: How to Generate and Configure SSH Certificate-Based Authentication in OpenSSH

#### **Overview Of SSH Certificates**

SSH certificate-based authentication enhances security compared to key-based
methods by:

- Associating certificates with user identity.
- Including metadata for role-based access control.
- Supporting expiration and imposing security constraints.
- Eliminating the "Trust on First Use" (TOFU) problem by using a Certificate
  Authority (CA).

#### **Key Steps to Implement SSH Certificates**

1. **Set Up a Certificate Authority (CA)**

   - Generate CA key pairs (host and user CAs) using ssh-keygen.
   - Protect the private keys to ensure security.

   ```bash
   ssh-keygen -t rsa -b 4096 -f host_ca -C host_ca
   ssh-keygen -t rsa -b 4096 -f user_ca -C user_ca
   ```

2. **Issue Certificates**

   - **Host Certificates:** Sign host keys to allow users to verify host
     identities.

   ```bash
   ssh-keygen -s host_ca -I host.example.com -h -n host.example.com -V +52w ssh_host_rsa_key.pub
   ```

   - **User Certificates:** Sign user keys to control access to hosts.

   ```bash
   ssh-keygen -s user_ca -I user@example.com -n username -V +1d user-key.pub
   ```

3. **Configure SSH for Host Certificate Authentication**

   - Copy the host certificate and CA public key to the server.
   - Add the following line to `/etc/ssh/sshd_config`:

     ```
     HostCertificate /etc/ssh/ssh_host_rsa_key-cert.pub
     ```

   - Restart the SSH daemon:

   ```bash
   systemctl restart sshd
   ```

4. **Configure SSH for User Certificate Authentication**

   - Copy the user CA public key to the server.
   - Add the following line to `/etc/ssh/sshd_config`:

     ```
     TrustedUserCAKeys /etc/ssh/user_ca.pub
     ```

   - Restart the SSH daemon:

   ```bash
   systemctl restart sshd
   ```

5. **Client-Side Configuration**

   - Add the host CA public key to the `~/.ssh/known_hosts` file:

     ```
     @cert-authority *.example.com <host_ca_public_key>
     ```

6. **Verification**

   - Ensure proper connectivity using certificates by checking logs:

   ```bash
   ssh -vv host.example.com
   ```

#### **Best Practices**

- Use separate CAs for hosts and users to minimize security risks.
- Define certificate expiration to enforce periodic renewal.
- Automate certificate issuance for efficiency and consistency.

### **Conclusion**

SSH certificate-based authentication significantly improves security by
incorporating identity, role-based access, and constraints. By setting up a
Certificate Authority and signing certificates, you eliminate common issues like
TOFU and outdated trust models.

# Application

# OpenSSH ED25519 PKI Management Script Synopsis

## Overview

The **OpenSSH ED25519 PKI Management** script is a comprehensive Bash tool
designed to simplify the configuration and management of SSH certificate-based
authentication. It provides a user-friendly GUI menu using `dialog`, enabling
administrators to efficiently handle Public Key Infrastructure (PKI) tasks for
OpenSSH on Debian 12 systems.

## Key Features

### 1. **Initial Configuration**

- **Directory Structure Setup:** Automatically creates the necessary directory
  hierarchy for Certificate Authorities (CA), including separate folders for
  hosts and users.
- **Revoked Keys Management:** Initializes an empty `revoked_keys` file to track
  revoked certificates.

### 2. **Signing Keys Management**

- **Create Signing Keys:** Generates ED25519 key pairs for both Host and User
  CAs.
- **Secure Storage:** Renames private keys with a `.private` extension and sets
  strict permissions to ensure security.

### 3. **Key Pair Generation**

- **User-Friendly Prompts:** Guides users to generate SSH key pairs by
  specifying an email address (for users) or FQDN (for hosts).
- **Secure Key Generation:** Creates key pairs without passphrases and secures
  private keys with appropriate permissions.

### 4. **Certificate Signing**

- **Flexible Signing:** Allows signing of both user and host keys with
  customizable principals and validity periods.
- **Validity Period Selection:** Offers predefined validity options ranging from
  1 hour to 10 years.
- **Additional Options:** Supports adding SSH certificate options such as
  disabling agent forwarding, port forwarding, PTY allocation, and more.
- **Automated Serial Number Generation:** Ensures each certificate has a unique
  serial number for tracking.

### 5. **Certificate Revocation**

- **Revoke Certificates:** Facilitates the revocation of certificates by
  updating the Certificate Revocation List (KRL).
- **Serial Number Management:** Maintains a serial number log for all revoked
  certificates.

### 6. **Advanced Management**

- **Clear CA Directories:** Provides a debug option to delete all CA-related
  directories (with confirmation prompts to prevent accidental data loss).
- **Display Directory Tree:** Visualizes the CA directory structure using the
  `tree` command within a dialog box.
- **Show Serial Numbers:** Lists all certificate serial numbers with prefixes
  indicating their type (Host or User).

### 7. **Information Display**

- **Public Key Display:** Retrieves and displays public keys based on user input
  (email or FQDN).
- **Certificate Information:** Shows detailed information about specific
  certificates, including their validity and associated principals.
- **Revoked Certificates List:** Presents a list of all revoked certificates
  from the KRL.

### 8. **User Interface**

- **GUI Menu:** Utilizes `dialog` to present intuitive menus and prompts, making
  it accessible even for users with limited command-line experience.
- **Confirmation Prompts:** Ensures critical actions are confirmed by the user
  to prevent unintended changes.
- **Operation Feedback:** Provides success and error messages to inform users
  about the outcome of their actions.

## Requirements

- **Operating System:** Tested on Debian 12 (amd64/arm64).
- **Privileges:** Must be run as Root \*
- **Dependencies:** Requires `coreutils`, `openssl`, `dialog`, and `tree`
  packages to be installed.

## Usage Notes

- **Non-X.509 Certificates:** Specifically designed for SSH use and does not
  handle X.509 certificates.
- **Private Code:** The script is marked as private and should not be
  distributed.
- **Caution Advised:** Users are warned to understand the script's operations
  before execution to prevent security risks.

## Conclusion

The OpenSSH ED25519 PKI Management script streamlines the process of setting up
and managing SSH certificate-based authentication. With its comprehensive
feature set and user-friendly interface, it enhances the security and efficiency
of SSH deployments by leveraging a robust PKI framework.
