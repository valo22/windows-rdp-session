# Temporary Windows RDP

A manually started Windows Server 2025 session using a public-repository runner. GitHub currently lists 4 vCPUs and 16 GB RAM for this runner. The job has a six-hour limit including setup. The default desktop session lasts 330 minutes after setup. Runner storage and availability are subject to GitHub's limits.

Use hosted runners for work permitted by [GitHub's Actions terms](https://docs.github.com/en/site-policy/github-terms/github-terms-for-additional-products-and-features), including development and testing of the repository's software.

## Setup

1. Install [Tailscale](https://tailscale.com/download) on the device you will connect from and sign in. Find that device's Tailscale IPv4 address in the app.
2. In your [Tailscale admin console](https://login.tailscale.com/admin/settings/keys), create a short-lived, one-use, ephemeral auth key for this runner. Your tailnet must permit the client to reach the runner on TCP port 3389. Approve the runner if your tailnet requires device approval.
3. In this repository, open Settings, then Secrets and variables, then Actions. Add two repository secrets.

| Secret name | Value |
|---|---|
| RDP_PASSWORD | A unique password at least 16 characters long that satisfies Windows password complexity requirements |
| TAILSCALE_AUTHKEY | The Tailscale auth key |

Enter these values directly into GitHub's secret forms. Keep the password available for your Windows sign-in. This public repository contains the workflow source.

## Start and connect

1. Open Actions, select Temporary Windows RDP, then select Run workflow.
2. Enter your client device's Tailscale IPv4 address and a session duration from 1 to 330 minutes. A five-minute run can be used for the initial connection test. A one-use auth key must be replaced before another run.
3. When Configure RDP over Tailscale finishes, read the address and Windows username in the run summary.
4. On your Tailscale-connected Windows client, open Remote Desktop Connection and connect to that address. Enter the username from the summary and the password you saved as RDP_PASSWORD.
5. Use Cancel workflow to stop early. Back up your files before the job ends.

The workflow requires Network Level Authentication and TLS. Its RDP firewall rule permits the supplied client address over the runner's Tailscale interface. The temporary account has administrator access to the runner. Cleanup logs the runner out of Tailscale and removes the account and firewall rule when it can execute. Each run gets a fresh VM. Files require a separate backup to survive job completion.

A successful listener check on the runner verifies that RDP started. A connection and Windows sign-in from your client are needed to verify end-to-end access.
