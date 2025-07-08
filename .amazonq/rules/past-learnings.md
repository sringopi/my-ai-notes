# Past Deployment Learnings and Rules

## Frontend Deployment Rules

### 1. Dynamic Configuration Rule
**Never hardcode environment-specific URLs in frontend code**
- Always use environment variables for API endpoints (e.g., `process.env.REACT_APP_API_URL`)
- Create `.env.example` files to document required environment variables
- Use separate `.env.production`, `.env.development` files for different environments
- Build frontend with environment-specific configurations at deployment time

### 2. SPA Routing Rule
**Single Page Applications require special routing configuration**
- For React Router apps, create a `_redirects` file in the public folder with: `/* /index.html 200`
- Configure nginx with try_files directive: `try_files $uri $uri/ /index.html`
- This ensures client-side routing works correctly after deployment

### 3. React Props Best Practices
**Avoid passing non-standard props to DOM elements**
- Use transient props with styled-components (prefix with `$`): `$status`, `$primary`, `$active`
- This prevents React warnings about unknown DOM properties
- Example: `<StyledDiv $primary={true}>` instead of `<StyledDiv primary={true}>`

### 4. Production Build Optimizations
**Always optimize React builds for production**
- Disable source maps in production: `GENERATE_SOURCEMAP=false npm run build`
- Use production environment variable: `NODE_ENV=production`
- Minify and compress assets
- Enable caching headers for static assets

### 5. Deployment Testing Rule
**Every deployment must have automated tests**
- Create Puppeteer or similar E2E tests to validate:
  - Frontend loads correctly
  - API endpoints are accessible
  - Critical user journeys work
  - Health checks pass
- Run tests immediately after deployment
- Include both UI tests and API tests

### 6. Build Artifact Rule
**Package applications properly for deployment**
- Create deployment packages (tar.gz) with all necessary files
- Include package.json for dependency installation
- Exclude unnecessary files (node_modules, .git, test files)
- Version deployment artifacts

## Infrastructure Rules

### 7. Health Check Configuration
**Always configure proper health checks**
- Backend health endpoint: `/api/health` returning 200 OK
- Frontend health check: Root path `/` returning 200 OK
- Set appropriate health check intervals and thresholds
- Health checks should verify actual application functionality, not just process existence

### 8. Auto-scaling Configuration
**Design for scalability from the start**
- Use Auto Scaling Groups even for single instances
- Configure scaling policies based on metrics (CPU, memory, request count)
- Set appropriate min/max/desired capacity
- Test scaling behavior under load

### 9. Monitoring and Logging
**Comprehensive monitoring is non-negotiable**
- Stream application logs to CloudWatch
- Set up alarms for critical metrics
- Monitor both infrastructure and application metrics
- Include deployment tracking

### 10. Security Best Practices
**Security must be built-in, not bolted-on**
- Use IAM roles for service authentication (never hardcode credentials)
- Enable SSL/TLS for all endpoints
- Implement WAF rules for public-facing applications
- Use Security Groups with least privilege access
- If deployed on cloud, store secrets in AWS Secrets Manager or Parameter Store or equivalent

## Testing Rules

### 11. Deployment Validation
**Every deployment must be validated**
- Test scripts should verify:
  - Application accessibility
  - API functionality
  - Database connectivity
  - External service integrations
- Automate rollback on validation failure

### 12. Local Testing Mirror
**Local development should mirror production**
- Use same Node.js version locally and in production
- Test with production-like environment variables
- Validate build process locally before deployment
- Use Docker to ensure consistency

## Next Steps for Any Production Deployment

1. **CI/CD Pipeline**: Automate build, test, and deployment processes
2. **CDN Integration**: Use CloudFront for static asset delivery
3. **SSL Certificates**: Implement HTTPS everywhere
4. **Backup Strategy**: Regular backups of data and configurations
5. **Disaster Recovery**: Document and test recovery procedures
6. **Performance Monitoring**: Set up APM (Application Performance Monitoring)
7. **Cost Optimization**: Monitor and optimize AWS resource usage

## Common Pitfalls to Avoid

1. **Assuming files exist**: Always check file existence before operations
2. **Hardcoding values**: Use environment variables and configuration files
3. **Ignoring error handling**: Implement comprehensive error handling
4. **Skipping tests**: Always test, even for "simple" changes
5. **Manual deployments**: Automate everything that can be automated
6. **Forgetting about state**: Consider application state during deployments
7. **Ignoring security**: Security should be considered at every step

## Deployment Checklist

- [ ] Environment variables configured
- [ ] Build optimizations enabled
- [ ] Health checks configured
- [ ] Monitoring set up
- [ ] Security groups reviewed
- [ ] SSL certificates installed
- [ ] Auto-scaling configured
- [ ] Backup strategy implemented
- [ ] Tests passing
- [ ] Documentation updated
