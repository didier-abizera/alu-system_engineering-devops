# Load Balancer

This directory contains Bash scripts related to load balancer configuration and custom HTTP response headers, completed as part of the ALU System Engineering & DevOps curriculum.# Load Balancer

This directory contains Bash scripts related to configuring web servers behind a load balancer, completed as part of the ALU System Engineering & DevOps curriculum.

## Project Overview

A load balancer distributes incoming traffic across multiple web servers, improving reliability and performance. Before setting up the actual load balancer, this project first configures two identical web servers (`web-01` and `web-02`) so that either one can serve requests interchangeably.

## Tasks

### 0. Double the number of webservers (`0-custom_http_response_header`)

This task has two parts:

1. **Configuring web-02 to match web-01**
   Using the Bash script written in the `web_server` project (`1-install_nginx_web_server`), Nginx was installed and configured identically on both `web-01` and `web-02`, so that both servers serve the same content.

2. **Adding a custom HTTP response header**
   Since both servers will eventually sit behind a load balancer, it becomes impossible to tell just from a webpage which server actually answered a request. To solve this, this script configures Nginx to include a custom header in every HTTP response:The value of this header is set dynamically using Nginx's built-in `$hostname` variable, so it automatically shows the correct server name (`7135-web-01` or `7135-web-02`) without hardcoding it — the same script works unmodified on either server.

   **What the script does, step by step:**
   - Updates the package list (`apt-get update`)
   - Installs Nginx (`apt-get install -y nginx`)
   - Inserts an `add_header X-Served-By $hostname;` directive into Nginx's default site configuration, right after the `server_name` line
   - Restarts Nginx so the new configuration takes effect

   **Testing:**
```bash
   curl -sI localhost | grep X-Served-By
```
   This confirms each server correctly reports its own hostname in the response headers.
