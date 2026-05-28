# WordPress on Kubernetes

Learning to deploy an app on Kubernetes with K3s

> [!TIP]
> Demo credentials only
> base64 encoded, not encrypted
> Do NOT use in production

## Troubleshooting

1. **Issue: Ingress returning 502 Bad Gateway** (Commit [d677f46](./commit/d677f46619dd0b267992fcdb2c6d55863156f755))
- Symptom: WordPress pod running, Ingress configured, 
  but return 502
- Checked: pod logs, service endpoints, Ingress logs, status all appear to be normal

- Root cause: Using `wordpress:7.0.0-php8.4-fpm` image which 
  exposes port 9000 (FastCGI) instead of 80 (HTTP). 
  Ingress expects HTTP but FPM requires NGINX as reverse proxy to work

- Fix: Revert to standard `wordpress:7.0.0` image (Apache bundled) fixes immediately
- Lesson: For lab cluster, standard image reduces complexity with no meaningful tradeoff, advoids over-engineering