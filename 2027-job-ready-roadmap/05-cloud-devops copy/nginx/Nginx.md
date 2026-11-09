# Nginx Reverse Proxy & Load Balancing – Important Topics & Learning Resources

## 1. Nginx Basics
- [ ] What is Nginx and its event-driven architecture
- [ ] Installing Nginx (apt / Docker)
- [ ] Config file structure (`nginx.conf`, `conf.d/`, `sites-enabled/`)
- [ ] Contexts: `main`, `events`, `http`, `server`, `location`
- [ ] `nginx -t` and `nginx -s reload`
- [ ] Access and error logs

## 2. Serving & Routing
- [ ] Serving static files (React build)
- [ ] `server_name` and virtual hosts
- [ ] `location` matching (prefix, exact, regex) and priority
- [ ] `try_files` for SPA routing
- [ ] Redirects and `rewrite`
- [ ] Gzip compression

## 3. Reverse Proxy
- [ ] `proxy_pass`
- [ ] Forwarding headers (`Host`, `X-Real-IP`, `X-Forwarded-For`, `X-Forwarded-Proto`)
- [ ] Spring Boot `server.forward-headers-strategy`
- [ ] Timeouts and buffering
- [ ] WebSocket proxying
- [ ] Proxying React frontend + Spring Boot API on one domain

## 4. Load Balancing
- [ ] `upstream` blocks
- [ ] Algorithms: round robin, least_conn, ip_hash, weights
- [ ] Health checks and `max_fails` / `fail_timeout`
- [ ] Sticky sessions vs stateless services
- [ ] Nginx vs cloud load balancers (ALB)

## 5. Security & Performance
- [ ] SSL/TLS termination
- [ ] Let's Encrypt with Certbot and auto-renewal
- [ ] HTTP → HTTPS redirect, HSTS
- [ ] Rate limiting (`limit_req`, `limit_conn`)
- [ ] Proxy caching (`proxy_cache`)
- [ ] Security headers and hiding version info
- [ ] Basic auth and IP allow/deny

## 6. Practical Tasks
- [ ] Nginx in Docker Compose in front of 3 Spring Boot instances
- [ ] Serve a React build and proxy `/api` to the backend
- [ ] Enable HTTPS with Let's Encrypt on an EC2 instance
- [ ] Add rate limiting to a login endpoint

## Important Learning Resources
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Nginx – Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)
- [Nginx – Reverse Proxy Admin Guide](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)
- [Nginx – HTTP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)
- [Certbot – Let's Encrypt](https://certbot.eff.org/)
