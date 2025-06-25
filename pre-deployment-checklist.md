# Pre-Deployment Checklist

## 1. Code Quality & Testing

### 1.1 Code Review & Static Analysis
**Task:** Perform comprehensive code review and static analysis
**AI Agent Prompt:** "Execute a complete code quality assessment including linting, formatting, type checking, and static analysis. Use IDE diagnostics to identify and report all code quality issues. Generate a detailed report with specific file locations and recommended fixes."
**Tool Calls:**
- `mcp__ide__getDiagnostics` - Get all language diagnostics for the project
- `Bash` commands:
  ```bash
  # ESLint for JavaScript/TypeScript
  npx eslint . --ext .js,.jsx,.ts,.tsx --format json
  
  # Prettier for formatting checks
  npx prettier --check . --write=false
  
  # TypeScript type checking
  npx tsc --noEmit --skipLibCheck
  
  # Python linting (if applicable)
  flake8 . --format=json
  pylint . --output-format=json
  mypy . --json-report
  ```
**Validation:** All diagnostic issues resolved, linting passes with zero errors, formatting is consistent

### 1.2 Automated Testing Suite
**Task:** Execute comprehensive test suite with coverage analysis
**AI Agent Prompt:** "Run all automated tests including unit tests, integration tests, and end-to-end tests. Generate coverage reports and identify any gaps in test coverage. Ensure all tests pass before deployment."
**Tool Calls:**
- `mcp__ide__executeCode` - Run test commands in appropriate environment
- `Bash` commands:
  ```bash
  # Node.js/JavaScript testing
  npm test -- --coverage --watchAll=false --ci
  npm run test:integration
  npm run test:e2e
  
  # Python testing
  pytest --cov=. --cov-report=json --cov-report=html
  python -m pytest tests/ -v --tb=short
  
  # Generate test reports
  npm run test:report || pytest --html=report.html
  ```
**Validation:** All tests pass, coverage meets minimum threshold (>80%), no flaky tests detected

### 1.3 Security Vulnerability Scanning
**Task:** Perform comprehensive security vulnerability assessment
**AI Agent Prompt:** "Execute security scans for known vulnerabilities in dependencies, code patterns, and configurations. Check for security best practices and identify potential security risks."
**Tool Calls:**
- `Bash` commands:
  ```bash
  # Node.js security audit
  npm audit --audit-level=moderate --json
  npx audit-ci --moderate
  
  # Python security scanning
  pip-audit --format=json
  bandit -r . -f json
  safety check --json
  
  # Generic security scanning
  npx eslint-plugin-security
  semgrep --config=auto --json
  ```
**Validation:** No high or critical vulnerabilities found, security recommendations implemented

### 1.4 Dependencies Audit & Management
**Task:** Audit and validate all project dependencies
**AI Agent Prompt:** "Analyze all project dependencies for security vulnerabilities, license compatibility, and version conflicts. Generate a report of outdated packages and recommended updates."
**Tool Calls:**
- `Bash` commands:
  ```bash
  # Node.js dependency analysis
  npm outdated --json
  npm ls --json
  npx license-checker --json
  
  # Python dependency analysis
  pip list --outdated --format=json
  pip-licenses --format=json
  pipdeptree --json
  
  # Check for dependency conflicts
  npm ls --depth=0 || pip check
  ```
**Validation:** All dependencies are up-to-date and secure, no version conflicts exist

## 2. Environment & Configuration

### 2.1 Environment Variables Validation
**Task:** Validate all environment variables and configuration settings
**AI Agent Prompt:** "Verify that all required environment variables are properly set, validate their formats and values, and ensure no sensitive data is exposed. Check environment-specific configurations for development, staging, and production."
**Tool Calls:**
- `mcp__ide__executeCode` - Run environment validation scripts
- `Bash` commands:
  ```bash
  # Environment variable validation
  printenv | grep -E "(API_KEY|DATABASE|SECRET|TOKEN|PASSWORD)" | wc -l
  
  # Validate .env files exist and are properly formatted
  [ -f .env ] && echo ".env exists" || echo ".env missing"
  [ -f .env.example ] && echo ".env.example exists" || echo ".env.example missing"
  
  # Check for environment-specific configs
  ls -la .env* || echo "No environment files found"
  
  # Validate required environment variables
  node -e "
    const requiredEnvs = ['NODE_ENV', 'PORT', 'DATABASE_URL'];
    const missing = requiredEnvs.filter(env => !process.env[env]);
    if (missing.length) {
      console.error('Missing environment variables:', missing);
      process.exit(1);
    }
    console.log('All required environment variables are set');
  "
  ```
**Validation:** All required environment variables are set and valid, no sensitive data exposed in version control

### 2.2 Configuration Files Review
**Task:** Review and validate all configuration files
**AI Agent Prompt:** "Examine all configuration files for correctness, consistency, and security. Validate JSON/YAML syntax, check for environment-specific overrides, and ensure no hardcoded secrets or sensitive information."
**Tool Calls:**
- `Glob` - Find all configuration files: `**/*.{json,yaml,yml,toml,ini,conf,config}`
- `Read` - Examine each configuration file
- `Bash` commands:
  ```bash
  # Validate JSON configuration files
  find . -name "*.json" -not -path "./node_modules/*" -exec jq empty {} \; 2>&1 | head -20
  
  # Validate YAML configuration files  
  find . -name "*.yml" -o -name "*.yaml" -not -path "./node_modules/*" -exec yamllint {} \; 2>&1 | head -20
  
  # Check for hardcoded secrets in config files
  grep -r -E "(password|secret|key|token).*[=:].*['\"][^'\"]{8,}" --include="*.json" --include="*.yml" --include="*.yaml" --exclude-dir=node_modules . || echo "No hardcoded secrets found"
  
  # Validate configuration file permissions
  find . -name "*.conf" -o -name "*.config" -o -name ".env*" -not -path "./node_modules/*" -exec ls -la {} \;
  ```
**Validation:** All configuration files are syntactically valid, no hardcoded secrets, proper file permissions set

### 2.3 Database Connection Testing
**Task:** Test all database connections and validate configurations
**AI Agent Prompt:** "Test connectivity to all configured databases, validate connection strings, check connection pooling settings, and verify database schemas are up-to-date. Test both read and write operations."
**Tool Calls:**
- `mcp__ide__executeCode` - Execute database connection tests
- `Bash` commands:
  ```bash
  # PostgreSQL connection test
  if [ -n "$DATABASE_URL" ]; then
    psql "$DATABASE_URL" -c "SELECT version();" || echo "PostgreSQL connection failed"
  fi
  
  # MySQL connection test
  if [ -n "$MYSQL_URL" ]; then
    mysql -e "SELECT VERSION();" --url="$MYSQL_URL" || echo "MySQL connection failed"
  fi
  
  # MongoDB connection test
  if [ -n "$MONGODB_URL" ]; then
    mongosh "$MONGODB_URL" --eval "db.adminCommand('ismaster')" || echo "MongoDB connection failed"
  fi
  
  # Redis connection test
  if [ -n "$REDIS_URL" ]; then
    redis-cli -u "$REDIS_URL" ping || echo "Redis connection failed"
  fi
  
  # Database migration status check
  npm run db:migrate:status || python manage.py showmigrations || echo "No migration system detected"
  ```
**Validation:** All database connections successful, migrations are up-to-date, connection pooling configured correctly

### 2.4 External API Integration Testing
**Task:** Test all external API integrations and third-party services
**AI Agent Prompt:** "Test connectivity and authentication with all external APIs and third-party services. Validate API keys, test rate limiting, and verify error handling for API failures."
**Tool Calls:**
- `mcp__fetch__fetch` - Test HTTP endpoints and APIs
- `Bash` commands for various API tests:
  ```bash
  # Test health endpoints
  curl -f -s -o /dev/null -w "%{http_code}" http://localhost:3000/health || echo "Health endpoint not responding"
  
  # Test external API connectivity (examples)
  if [ -n "$STRIPE_API_KEY" ]; then
    curl -H "Authorization: Bearer $STRIPE_API_KEY" https://api.stripe.com/v1/account
  fi
  
  if [ -n "$SENDGRID_API_KEY" ]; then
    curl -H "Authorization: Bearer $SENDGRID_API_KEY" https://api.sendgrid.com/v3/user/account
  fi
  
  # Test webhook endpoints
  curl -X POST -H "Content-Type: application/json" -d '{"test": true}' http://localhost:3000/webhook/test
  ```
**Validation:** All external APIs respond correctly, authentication is working, rate limits are respected

### 2.5 Secrets Management Verification
**Task:** Verify secure handling of secrets and sensitive configuration
**AI Agent Prompt:** "Audit all secrets management practices, ensure sensitive data is properly encrypted and stored, validate access controls, and check for any exposed secrets in logs or error messages."
**Tool Calls:**
- `Grep` - Search for potential secret exposures: `(api[_-]?key|secret|password|token|private[_-]?key)`
- `Bash` commands:
  ```bash
  # Check for secrets in git history
  git log --all --grep="password\|secret\|key" --oneline | head -10
  
  # Search for potential secrets in codebase
  rg -i "(api[_-]?key|secret|password|token|private[_-]?key).*[=:].*['\"][a-zA-Z0-9]{20,}" --type-not=lock
  
  # Validate secrets are not in environment files committed to git
  git ls-files | xargs grep -l "SECRET\|PASSWORD\|API_KEY" | grep -v ".env.example" || echo "No committed secrets found"
  
  # Check file permissions on sensitive files
  find . -name ".env*" -o -name "*secret*" -o -name "*key*" -not -path "./node_modules/*" -exec ls -la {} \;
  
  # Validate secrets rotation policy
  echo "Last password change date should be checked manually for each service"
  ```
**Validation:** No secrets exposed in code or git history, proper access controls in place, secrets rotation policy documented

## 3. Docker & Containerization

### 3.1 Dockerfile Optimization
**Task:** Analyze and optimize Dockerfile for production deployment
**AI Agent Prompt:** "Examine the project's Dockerfile and optimize it for production use. Focus on reducing image size, improving build cache efficiency, and implementing security best practices. Create a multi-stage build if not already present."
**Tool Calls:**
```bash
# Analyze current Dockerfile
docker build --no-cache -t temp-image .
docker images temp-image
docker history temp-image --human --format "table {{.CreatedBy}}\t{{.Size}}"

# Check for security vulnerabilities in base image
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(pwd):/tmp/app aquasec/trivy image temp-image

# Optimize build cache
docker build --target development -t app:dev .
docker build --target production -t app:prod .
```
**Validation:** Image size reduced by at least 30%, security scan shows no HIGH/CRITICAL vulnerabilities, build cache utilization improved

### 3.2 Multi-stage Build Verification
**Task:** Verify multi-stage build configuration and layer optimization
**AI Agent Prompt:** "Validate that the Dockerfile uses multi-stage builds effectively. Ensure development dependencies are excluded from production image, and verify that each stage serves its intended purpose."
**Tool Calls:**
```bash
# Build and inspect each stage
docker build --target development -t app:dev .
docker build --target production -t app:prod .

# Compare image sizes
docker images | grep "app:"

# Inspect layer differences
docker history app:dev --format "table {{.CreatedBy}}\t{{.Size}}"
docker history app:prod --format "table {{.CreatedBy}}\t{{.Size}}"

# Verify production image doesn't contain dev dependencies
docker run --rm app:prod ls -la /app/node_modules 2>/dev/null || echo "Dev dependencies properly excluded"
```
**Validation:** Production image is significantly smaller than development image, no development dependencies in production image, each stage builds successfully

### 3.3 Image Security Scanning
**Task:** Perform comprehensive security scanning on container images
**AI Agent Prompt:** "Execute thorough security scans on both development and production Docker images. Identify and document all vulnerabilities, provide remediation steps for critical issues."
**Tool Calls:**
```bash
# Install and run Trivy scanner
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(pwd):/tmp/app aquasec/trivy image --exit-code 1 --severity HIGH,CRITICAL app:prod

# Scan for secrets and sensitive data
docker run --rm -v $(pwd):/app trufflesecurity/trufflehog:latest filesystem /app --json

# Check for known malware
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  clamav/clamav:stable clamscan --infected --recursive /var/lib/docker
```
**Validation:** No HIGH or CRITICAL vulnerabilities found, no secrets exposed in image layers, malware scan passes

### 3.4 Container Resource Limits Configuration
**Task:** Configure appropriate resource limits and requests for containers
**AI Agent Prompt:** "Analyze the application's resource usage patterns and configure appropriate CPU and memory limits. Ensure containers have resource requests set for proper scheduling and limits set to prevent resource starvation."
**Tool Calls:**
```bash
# Monitor resource usage during testing
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.MemPerc}}" --no-stream

# Test with resource limits
docker run --memory="512m" --cpus="1.0" --name resource-test app:prod

# Validate limits are enforced
docker exec resource-test cat /sys/fs/cgroup/memory/memory.limit_in_bytes
docker exec resource-test nproc --all
```
**Validation:** Resource limits prevent container from consuming excessive resources, application runs within defined limits, proper resource requests configured

### 3.5 Health Checks Configuration
**Task:** Implement and verify container health checks
**AI Agent Prompt:** "Configure Docker health checks for the application container. Implement both startup and liveness probes that accurately reflect application health status."
**Tool Calls:**
```bash
# Test health check endpoint
curl -f http://localhost:3000/health || exit 1

# Build image with health check
docker build -t app:health .

# Run container and monitor health status
docker run -d --name health-test app:health
docker ps --format "table {{.Names}}\t{{.Status}}"

# Verify health check behavior
docker exec health-test curl -f localhost:3000/health
docker inspect health-test --format='{{.State.Health.Status}}'
```
**Validation:** Health checks respond correctly during normal operation, unhealthy containers are detected and marked appropriately

### 3.6 Test Deployment with Puppeteer Validation
**Task:** Deploy container in isolated environment and validate functionality using automated browser testing
**AI Agent Prompt:** "Deploy the containerized application in an isolated test environment and use Puppeteer to validate that all critical user journeys work correctly. Test authentication, core features, and API endpoints."
**Tool Calls:**
```bash
# Deploy container in test environment
docker run -d -p 3000:3000 --name app-test app:prod

# Wait for container to be ready
sleep 10

# Use Puppeteer MCP to validate functionality
```
**MCP Tool Calls:**
- `mcp__puppeteer__puppeteer_navigate`: Navigate to http://localhost:3000
- `mcp__puppeteer__puppeteer_screenshot`: Take screenshot of homepage
- `mcp__puppeteer__puppeteer_click`: Test navigation and interactive elements
- `mcp__puppeteer__puppeteer_fill`: Test form submissions and user input
- `mcp__puppeteer__puppeteer_evaluate`: Execute JavaScript to verify API responses

**Validation:** All critical user journeys complete successfully, no JavaScript errors in console, API endpoints respond correctly, container starts and stops gracefully

## 4. Database & Data Management

### 4.1 Database Migration Testing
**Task:** Verify database migrations work correctly in production-like environment
**AI Agent Prompt:** "Test all database migrations in a production-like environment. Verify that migrations can be applied, rolled back, and that data integrity is maintained throughout the process."
**Tool Calls:**
```bash
# Create test database backup
pg_dump -U postgres production_db > migration_test_backup.sql

# Apply migrations to test database
npm run migrate:up
# or
python manage.py migrate

# Verify migration status
npm run migrate:status
# or
python manage.py showmigrations

# Test rollback capability
npm run migrate:down
python manage.py migrate previous_migration_name

# Restore and test with real data
psql -U postgres test_db < migration_test_backup.sql
npm run migrate:up
```
**MCP Tool Calls:**
- `mcp__sqlite__list_tables`: List all database tables
- `mcp__sqlite__describe_table`: Inspect table schema changes
- `mcp__sqlite__read_query`: Verify data integrity after migrations

**Validation:** All migrations apply successfully, rollback works correctly, no data loss occurs, schema changes are properly implemented

### 4.2 Backup Verification and Recovery Testing
**Task:** Verify backup procedures and test recovery processes
**AI Agent Prompt:** "Validate that database backup procedures work correctly and test the full recovery process. Ensure backups are consistent, complete, and can be restored successfully."
**Tool Calls:**
```bash
# Create and verify backup
pg_dump -U postgres -h localhost -p 5432 production_db > backup_$(date +%Y%m%d_%H%M%S).sql

# Verify backup integrity
pg_restore --list backup_$(date +%Y%m%d_%H%M%S).sql

# Test recovery process
createdb test_recovery_db
psql -U postgres test_recovery_db < backup_$(date +%Y%m%d_%H%M%S).sql

# Verify restored data
psql -U postgres test_recovery_db -c "SELECT COUNT(*) FROM users;"
psql -U postgres test_recovery_db -c "SELECT COUNT(*) FROM orders;"

# Test point-in-time recovery (if applicable)
pg_basebackup -D /tmp/backup -Ft -z -P
```
**MCP Tool Calls:**
- `mcp__sqlite__read_query`: Verify data consistency in restored database
- `mcp__sqlite__write_query`: Test write operations on restored database

**Validation:** Backups complete without errors, restoration process works correctly, restored data matches original, automated backup schedule is configured

### 4.3 Data Integrity Verification
**Task:** Perform comprehensive data integrity checks
**AI Agent Prompt:** "Execute thorough data integrity checks including foreign key constraints, data type validation, and business rule compliance. Identify and report any data inconsistencies."
**Tool Calls:**
```bash
# Check foreign key constraints
psql -U postgres production_db -c "SELECT * FROM information_schema.table_constraints WHERE constraint_type = 'FOREIGN KEY';"

# Verify data consistency
psql -U postgres production_db -c "SELECT COUNT(*) FROM users WHERE email IS NULL OR email = '';"
psql -U postgres production_db -c "SELECT COUNT(*) FROM orders WHERE user_id NOT IN (SELECT id FROM users);"

# Check for duplicate records
psql -U postgres production_db -c "SELECT email, COUNT(*) FROM users GROUP BY email HAVING COUNT(*) > 1;"

# Validate data types and formats
psql -U postgres production_db -c "SELECT COUNT(*) FROM users WHERE email !~ '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$';"
```
**MCP Tool Calls:**
- `mcp__sqlite__read_query`: Check for orphaned records and constraint violations
- `mcp__sqlite__read_query`: Validate business rules and data consistency
- `mcp__sqlite__append_insight`: Document data quality findings

**Validation:** No constraint violations found, data types are correct, business rules are enforced, duplicate detection works correctly

### 4.4 Connection Pooling Configuration Validation
**Task:** Verify database connection pooling is properly configured and optimized
**AI Agent Prompt:** "Validate that database connection pooling is configured correctly for production load. Test connection limits, timeout settings, and pool performance under various load conditions."
**Tool Calls:**
```bash
# Check current connection pool settings
psql -U postgres production_db -c "SHOW max_connections;"
psql -U postgres production_db -c "SELECT COUNT(*) FROM pg_stat_activity;"

# Test connection pool under load
for i in {1..50}; do
  psql -U postgres production_db -c "SELECT 1;" &
done
wait

# Monitor connection pool metrics
psql -U postgres production_db -c "SELECT datname, numbackends, xact_commit, xact_rollback FROM pg_stat_database WHERE datname = 'production_db';"

# Verify connection pool configuration in application
grep -r "pool" config/database.yml
cat .env | grep -i "pool\|connection"
```
**MCP Tool Calls:**
- `mcp__sqlite__read_query`: Monitor connection statistics and pool utilization

**Validation:** Connection pool handles expected load, proper timeout settings configured, no connection leaks detected, pool size optimized for application needs

### 4.5 Performance Optimization Verification
**Task:** Verify database performance optimizations are applied and effective
**AI Agent Prompt:** "Validate that all database performance optimizations are properly implemented. Check indexes, query performance, and database configuration settings for production readiness."
**Tool Calls:**
```bash
# Check database indexes
psql -U postgres production_db -c "SELECT schemaname, tablename, indexname, indexdef FROM pg_indexes WHERE schemaname = 'public' ORDER BY tablename, indexname;"

# Analyze query performance
psql -U postgres production_db -c "EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';"
psql -U postgres production_db -c "EXPLAIN ANALYZE SELECT COUNT(*) FROM orders WHERE created_at > NOW() - INTERVAL '30 days';"

# Check for unused indexes
psql -U postgres production_db -c "SELECT schemaname, tablename, attname, n_distinct, most_common_vals FROM pg_stats WHERE schemaname = 'public';"

# Verify database configuration
psql -U postgres production_db -c "SELECT name, setting, unit FROM pg_settings WHERE name IN ('shared_buffers', 'work_mem', 'maintenance_work_mem', 'effective_cache_size');"

# Check table statistics
psql -U postgres production_db -c "SELECT schemaname, tablename, n_tup_ins, n_tup_upd, n_tup_del FROM pg_stat_user_tables;"
```
**MCP Tool Calls:**
- `mcp__sqlite__read_query`: Analyze query execution plans and performance metrics
- `mcp__sqlite__append_insight`: Document performance optimization findings

**Validation:** All critical queries use appropriate indexes, query execution times meet performance targets, database configuration optimized for workload, no missing indexes identified

## 5. Infrastructure & Deployment

### 5.1 CapRover Server Setup Verification
**Task:** Verify CapRover server is properly configured and accessible
**AI Agent Prompt:** "Verify that the CapRover server is running and properly configured. Check the server status, validate the initial setup, and ensure all required ports are accessible. Use web fetch tools to test server connectivity and dashboard accessibility."
**Tool Calls:**
- `mcp__fetch__fetch` to check CapRover dashboard accessibility at `https://captain.your-domain.com`
- `mcp__puppeteer__puppeteer_navigate` to access CapRover dashboard
- `mcp__puppeteer__puppeteer_screenshot` to capture dashboard state
**Validation:** 
- [ ] CapRover dashboard loads without errors
- [ ] Server shows "Healthy" status
- [ ] All system checks pass in dashboard
- [ ] Required ports (80, 443, 3000, 996, 7946, 4789, 2377) are accessible

### 5.2 DNS Configuration Validation
**Task:** Validate DNS configuration and wildcard A-record setup
**AI Agent Prompt:** "Verify that DNS configuration is correct for CapRover deployment. Check that the wildcard A-record is properly configured and resolving to the correct server IP. Test both root domain and wildcard subdomain resolution."
**Tool Calls:**
- `mcp__fetch__fetch` to test domain resolution: `https://your-domain.com`
- `mcp__fetch__fetch` to test wildcard subdomain: `https://test.your-domain.com`
- `Bash` with `dig` or `nslookup` commands to verify DNS records
**Validation:**
- [ ] Root domain resolves to correct server IP
- [ ] Wildcard A-record (*.your-domain.com) resolves correctly
- [ ] DNS propagation completed globally
- [ ] No DNS resolution errors or timeouts

### 5.3 SSL/TLS Certificate Verification
**Task:** Verify SSL/TLS certificates are properly configured and valid
**AI Agent Prompt:** "Check SSL/TLS certificate configuration for both the CapRover dashboard and application domains. Verify certificate validity, chain completeness, and HTTPS enforcement settings."
**Tool Calls:**
- `mcp__fetch__fetch` with HTTPS URLs to verify certificate validity
- `mcp__puppeteer__puppeteer_navigate` to test HTTPS redirects
- `Bash` with `openssl` commands to check certificate details
**Validation:**
- [ ] CapRover dashboard certificate is valid and trusted
- [ ] HTTPS enforced on CapRover dashboard
- [ ] Application domains have valid certificates
- [ ] Certificate chain is complete
- [ ] No SSL/TLS warnings or errors

### 5.4 Captain-definition File Validation
**Task:** Validate captain-definition file syntax and configuration
**AI Agent Prompt:** "Review and validate the captain-definition file in the project root. Ensure proper JSON syntax, correct schema version, and appropriate configuration for the application type. Verify compatibility with CapRover deployment requirements."
**Tool Calls:**
- `Read` to examine captain-definition file
- `Bash` with `jq` to validate JSON syntax
- `mcp__context7__get-library-docs` for CapRover documentation reference
**Validation:**
- [ ] captain-definition file exists in project root
- [ ] JSON syntax is valid
- [ ] Schema version is correct (version 2 recommended)
- [ ] Configuration matches application requirements
- [ ] No deprecated or invalid configuration options

### 5.5 CapRover CLI Authentication Testing
**Task:** Test CapRover CLI authentication and server connectivity
**AI Agent Prompt:** "Verify that CapRover CLI is properly installed and authenticated with the target server. Test CLI commands and ensure deployment capabilities are functional."
**Tool Calls:**
- `Bash` to check CapRover CLI installation: `caprover --version`
- `Bash` to test server connectivity: `caprover list`
- `Bash` to verify authentication status
**Validation:**
- [ ] CapRover CLI is installed and updated
- [ ] Authentication with server successful
- [ ] CLI can list apps and server information
- [ ] Deployment permissions verified
- [ ] No authentication or connectivity errors

### 5.6 Domain Configuration Testing
**Task:** Test application domain configuration and accessibility
**AI Agent Prompt:** "Verify that application domains are properly configured in CapRover and accessible. Test both default CapRover domains and custom domains, ensuring proper HTTP to HTTPS redirection."
**Tool Calls:**
- `mcp__fetch__fetch` to test default CapRover domain
- `mcp__fetch__fetch` to test custom domains
- `mcp__firecrawl__firecrawl_scrape` to verify application response
- `mcp__puppeteer__puppeteer_navigate` to test domain accessibility
**Validation:**
- [ ] Default CapRover domain accessible
- [ ] Custom domains properly configured
- [ ] HTTP to HTTPS redirection working
- [ ] Application loads correctly on all domains
- [ ] No domain resolution or accessibility issues

## 6. Monitoring & Observability

### 6.1 Logging Configuration Verification
**Task:** Verify application logging is properly configured and accessible
**AI Agent Prompt:** "Verify that application logging is properly configured and logs are accessible through CapRover dashboard and Docker service logs. Check log levels, format, and retention settings. Ensure logs provide sufficient information for debugging and monitoring."
**Tool Calls:**
- `Bash` to check Docker service logs: `docker service logs srv-captain--app-name --follow --tail 50`
- `mcp__puppeteer__puppeteer_navigate` to access CapRover app logs section
- `mcp__puppeteer__puppeteer_screenshot` to capture log output
- `Read` to check application logging configuration files
**Validation:**
- [ ] Application logs accessible via CapRover dashboard
- [ ] Docker service logs showing recent entries
- [ ] Log format includes timestamps and severity levels
- [ ] No logging errors or missing log entries
- [ ] Log retention policy configured appropriately
- [ ] Sensitive information not logged

### 6.2 Health Check Endpoint Testing
**Task:** Test application health check endpoints and monitoring
**AI Agent Prompt:** "Test the application's health check endpoints to ensure they respond correctly and provide meaningful status information. Verify that health checks are accessible both internally and externally, and validate response formats."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_scrape` to test health endpoint: `https://your-app.com/health`
- `mcp__fetch__fetch` to verify health check response format
- `mcp__puppeteer__puppeteer_navigate` to test health endpoint accessibility
- `Bash` with `curl` to test health endpoint response codes
**Validation:**
- [ ] Health check endpoint returns 200 OK status
- [ ] Health response includes application status information
- [ ] Database connectivity status included (if applicable)
- [ ] External dependencies status checked
- [ ] Response time within acceptable limits
- [ ] Health endpoint accessible from monitoring systems

### 6.3 Error Tracking Setup Validation
**Task:** Verify error tracking and reporting mechanisms are functional
**AI Agent Prompt:** "Validate that error tracking systems are properly configured and capturing application errors. Test error reporting, notification systems, and error aggregation. Ensure errors are properly categorized and actionable."
**Tool Calls:**
- `Read` to examine error tracking configuration files
- `mcp__fetch__fetch` to test error tracking service endpoints
- `Bash` to check error tracking service connectivity
- `mcp__firecrawl__firecrawl_scrape` to verify error dashboard accessibility
**Validation:**
- [ ] Error tracking service configured and connected
- [ ] Test errors properly captured and reported
- [ ] Error notifications functioning
- [ ] Error categorization and severity levels set
- [ ] Error tracking dashboard accessible
- [ ] Historical error data preserved

### 6.4 Performance Monitoring Configuration
**Task:** Verify performance monitoring and metrics collection setup
**AI Agent Prompt:** "Validate that performance monitoring systems are configured and collecting relevant metrics. Check response times, resource utilization, and application performance indicators. Ensure monitoring data is accessible and actionable."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_scrape` to test monitoring dashboard accessibility
- `mcp__fetch__fetch` to verify metrics endpoints
- `Bash` to check system resource monitoring commands
- `mcp__puppeteer__puppeteer_navigate` to access monitoring interfaces
**Validation:**
- [ ] Performance metrics being collected
- [ ] Response time monitoring active
- [ ] Resource utilization tracking (CPU, memory, disk)
- [ ] Database performance monitoring (if applicable)
- [ ] Custom application metrics configured
- [ ] Monitoring dashboard accessible and functional
- [ ] Historical performance data available

### 6.5 Alerting Rules Configuration
**Task:** Verify alerting rules and notification systems are properly configured
**AI Agent Prompt:** "Test alerting rules configuration to ensure proper notifications for critical issues. Verify alert thresholds, notification channels, and escalation procedures. Test both critical and warning level alerts."
**Tool Calls:**
- `Read` to examine alerting configuration files
- `mcp__fetch__fetch` to test alerting service endpoints
- `Bash` to test notification delivery mechanisms
- `mcp__firecrawl__firecrawl_scrape` to verify alerting dashboard
**Validation:**
- [ ] Alert rules configured for critical metrics
- [ ] Notification channels tested and functional
- [ ] Alert thresholds set appropriately
- [ ] Escalation procedures documented and tested
- [ ] Alert suppression rules configured
- [ ] Alert acknowledgment system working
- [ ] Test alerts delivered successfully

### 6.6 Comprehensive Monitoring Integration Testing
**Task:** Test end-to-end monitoring and observability integration
**AI Agent Prompt:** "Perform comprehensive testing of the entire monitoring stack. Verify that all monitoring components work together correctly, data flows properly between systems, and the overall observability provides complete visibility into application health and performance."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_scrape` to access all monitoring dashboards
- `mcp__fetch__fetch` to test all monitoring endpoints
- `Bash` to generate test load and verify monitoring response
- `mcp__puppeteer__puppeteer_navigate` to test monitoring workflow
**Validation:**
- [ ] All monitoring systems integrated and communicating
- [ ] End-to-end monitoring workflow functional
- [ ] Monitoring data consistency across systems
- [ ] Correlation between logs, metrics, and traces
- [ ] Monitoring system performance acceptable
- [ ] Backup monitoring systems configured
- [ ] Monitoring documentation updated and accessible

## 7. Security & Compliance

### 7.1 Authentication/Authorization Testing
**Task:** Verify authentication flows and authorization controls work correctly across all user roles and protected endpoints.

**AI Agent Prompt:** Test the application's authentication and authorization mechanisms by simulating various user scenarios. Navigate through login flows, test protected routes, verify role-based access controls, and ensure unauthorized access is properly blocked. Document any security vulnerabilities or bypass attempts.

**Tool Calls:**
```bash
# Navigate to application and test auth flows
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
mcp__puppeteer__puppeteer_screenshot(name="login_page", width=1200, height=800)

# Test login functionality
mcp__puppeteer__puppeteer_fill(selector="input[name='username']", value="test_user")
mcp__puppeteer__puppeteer_fill(selector="input[name='password']", value="test_password")
mcp__puppeteer__puppeteer_click(selector="button[type='submit']")
mcp__puppeteer__puppeteer_screenshot(name="authenticated_dashboard", width=1200, height=800)

# Test protected routes without authentication
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com/admin")
mcp__puppeteer__puppeteer_screenshot(name="unauthorized_access_attempt", width=1200, height=800)

# Verify logout functionality
mcp__puppeteer__puppeteer_click(selector="[data-testid='logout-button']")
mcp__puppeteer__puppeteer_screenshot(name="logout_confirmation", width=1200, height=800)
```

**Validation:** 
- Login redirects to dashboard for valid credentials
- Invalid credentials show appropriate error messages
- Protected routes redirect to login when unauthenticated
- User roles restrict access to appropriate sections
- Logout properly clears session and redirects

### 7.2 Security Headers Validation
**Task:** Verify all essential security headers are properly configured to protect against common web vulnerabilities.

**AI Agent Prompt:** Fetch the application's main pages and analyze HTTP response headers for security configurations. Check for presence and proper configuration of Content Security Policy, X-Frame-Options, X-Content-Type-Options, Strict-Transport-Security, and other security headers. Report any missing or misconfigured headers.

**Tool Calls:**
```bash
# Check main application security headers
mcp__fetch__fetch(url="https://your-app-domain.com", raw=true)

# Check API endpoints security headers
mcp__fetch__fetch(url="https://your-app-domain.com/api/health", raw=true)

# Check admin routes security headers
mcp__fetch__fetch(url="https://your-app-domain.com/admin", raw=true)

# Verify HTTPS enforcement
mcp__fetch__fetch(url="http://your-app-domain.com", raw=true)
```

**Validation:**
- Content-Security-Policy header present and restrictive
- X-Frame-Options set to DENY or SAMEORIGIN
- X-Content-Type-Options set to nosniff
- Strict-Transport-Security header present for HTTPS
- X-XSS-Protection header configured
- HTTP requests redirect to HTTPS
- No sensitive information leaked in headers

### 7.3 Rate Limiting Testing
**Task:** Verify rate limiting is properly implemented to prevent abuse and DOS attacks.

**AI Agent Prompt:** Test the application's rate limiting by making rapid successive requests to various endpoints. Monitor response codes, headers, and timing to verify rate limits are enforced. Test both authenticated and unauthenticated endpoints, and document the rate limiting behavior.

**Tool Calls:**
```bash
# Test rate limiting on public endpoints
for i in range(100):
    mcp__fetch__fetch(url="https://your-app-domain.com/api/public")

# Test rate limiting on authenticated endpoints
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com/login")
# Login first
mcp__puppeteer__puppeteer_fill(selector="input[name='username']", value="test_user")
mcp__puppeteer__puppeteer_fill(selector="input[name='password']", value="test_password")
mcp__puppeteer__puppeteer_click(selector="button[type='submit']")

# Execute rapid requests via JavaScript
mcp__puppeteer__puppeteer_evaluate(script=`
  const results = [];
  for(let i = 0; i < 50; i++) {
    fetch('/api/protected-endpoint').then(r => results.push({status: r.status, headers: Object.fromEntries(r.headers)}));
  }
  return results;
`)
```

**Validation:**
- Rate limiting returns 429 status code when exceeded
- Rate-limit headers present (X-RateLimit-Limit, X-RateLimit-Remaining)
- Different limits for authenticated vs unauthenticated users
- Rate limits reset after specified time window
- Critical endpoints (login, password reset) have strict limits

### 7.4 CORS Policies Validation
**Task:** Verify Cross-Origin Resource Sharing (CORS) policies are properly configured to allow legitimate requests while blocking unauthorized origins.

**AI Agent Prompt:** Test CORS configuration by making requests from different origins to your API endpoints. Verify that allowed origins can access resources while unauthorized origins are blocked. Check preflight requests for complex operations and ensure CORS headers are properly set.

**Tool Calls:**
```bash
# Test CORS preflight requests
mcp__fetch__fetch(url="https://your-app-domain.com/api/data", raw=true)

# Simulate cross-origin requests via browser
mcp__puppeteer__puppeteer_navigate(url="https://httpbin.org/")
mcp__puppeteer__puppeteer_evaluate(script=`
  fetch('https://your-app-domain.com/api/data', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({test: 'data'})
  }).then(r => ({status: r.status, headers: Object.fromEntries(r.headers)}))
`)

# Test from different origins
mcp__puppeteer__puppeteer_evaluate(script=`
  fetch('https://your-app-domain.com/api/public', {
    method: 'GET',
    headers: {'Origin': 'https://malicious-site.com'}
  }).then(r => r.headers.get('Access-Control-Allow-Origin'))
`)
```

**Validation:**
- Access-Control-Allow-Origin header properly configured
- Preflight requests return appropriate CORS headers
- Unauthorized origins receive CORS errors
- Credentials handling configured correctly
- Allowed methods and headers match application needs

### 7.5 CapRover Firewall Configuration Verification
**Task:** Verify CapRover server firewall is properly configured with only necessary ports open.

**AI Agent Prompt:** Check that the CapRover server has proper firewall configuration with only required ports accessible. Verify ports 80 (HTTP), 443 (HTTPS), 3000 (CapRover dashboard), 996 (CapRover secure), 7946 (Docker Swarm), 4789 (Docker overlay), and 2377 (Docker Swarm) are properly configured and no unnecessary ports are exposed.

**Tool Calls:**
```bash
# Test required CapRover ports accessibility
mcp__fetch__fetch(url="http://your-caprover-domain.com:80")
mcp__fetch__fetch(url="https://your-caprover-domain.com:443")  
mcp__fetch__fetch(url="https://captain.your-caprover-domain.com:3000")

# Test that non-essential ports are blocked (these should fail)
mcp__fetch__fetch(url="http://your-caprover-domain.com:22")  # SSH should be blocked from web
mcp__fetch__fetch(url="http://your-caprover-domain.com:3306") # MySQL should be blocked
mcp__fetch__fetch(url="http://your-caprover-domain.com:5432") # PostgreSQL should be blocked
```

**Validation:**
- Ports 80, 443, 3000, 996, 7946, 4789, 2377 are accessible as needed
- Unnecessary ports (SSH, database ports, etc.) are not publicly accessible
- CapRover dashboard redirects HTTP to HTTPS
- Only authorized IP ranges can access sensitive ports

### 7.6 HTTPS Enforcement Testing
**Task:** Verify HTTPS is properly enforced across all application endpoints and CapRover dashboard.

**AI Agent Prompt:** Test HTTPS enforcement by attempting to access all application URLs via HTTP and verifying they redirect to HTTPS. Check SSL certificate validity, ensure no mixed content warnings, and verify HTTPS is enforced on the CapRover dashboard.

**Tool Calls:**
```bash
# Test HTTP to HTTPS redirects
mcp__fetch__fetch(url="http://your-app-domain.com")
mcp__fetch__fetch(url="http://www.your-app-domain.com")
mcp__fetch__fetch(url="http://captain.your-caprover-domain.com")

# Test HTTPS certificate validity
mcp__fetch__fetch(url="https://your-app-domain.com")
mcp__fetch__fetch(url="https://captain.your-caprover-domain.com")

# Check for mixed content issues
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
mcp__puppeteer__puppeteer_evaluate(script=`
  // Check for mixed content warnings
  const resources = performance.getEntriesByType('resource');
  return resources.filter(r => r.name.startsWith('http:'));
`)
mcp__puppeteer__puppeteer_screenshot(name="https_enforcement_check", width=1200, height=800)
```

**Validation:**
- All HTTP requests redirect to HTTPS (301/302 status)
- SSL certificates are valid and not expired
- No mixed content warnings in browser console
- HSTS header properly configured
- CapRover dashboard only accessible via HTTPS

## 8. Performance & Scalability

### 8.1 Load Testing Using Puppeteer
**Task:** Perform comprehensive load testing to validate application performance under concurrent user scenarios.

**AI Agent Prompt:** Simulate multiple concurrent users accessing the application to test performance under load. Navigate through critical user journeys, measure response times, monitor for errors, and validate that the application maintains acceptable performance levels. Focus on authentication flows, data-heavy pages, and API endpoints that are likely to be bottlenecks.

**Tool Calls:**
```bash
# Initialize performance monitoring
mcp__sqlite__create_table(query="CREATE TABLE IF NOT EXISTS performance_metrics (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  test_type TEXT,
  url TEXT,
  response_time_ms INTEGER,
  status_code INTEGER,
  error_message TEXT,
  concurrent_users INTEGER
)")

# Load test - Homepage performance
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
start_time = performance.now()
mcp__puppeteer__puppeteer_screenshot(name="load_test_homepage", width=1200, height=800)
end_time = performance.now()

# Store homepage load time
mcp__sqlite__write_query(query="INSERT INTO performance_metrics (test_type, url, response_time_ms, concurrent_users) 
  VALUES ('homepage_load', 'https://your-app-domain.com', {}, 1)".format(int(end_time - start_time)))

# Simulate concurrent user login flows
mcp__puppeteer__puppeteer_evaluate(script=`
  const concurrentLogins = async (userCount = 10) => {
    const results = [];
    const promises = [];
    
    for(let i = 0; i < userCount; i++) {
      const promise = fetch('/api/login', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({username: 'test_user_' + i, password: 'test_password'})
      }).then(r => ({
        userId: i,
        status: r.status,
        responseTime: performance.now(),
        headers: Object.fromEntries(r.headers)
      }));
      promises.push(promise);
    }
    
    return Promise.all(promises);
  };
  
  return concurrentLogins(10);
`)

# Load test critical API endpoints
for endpoint in ['/api/data', '/api/users', '/api/dashboard']:
    mcp__puppeteer__puppeteer_evaluate(script=f`
      const testEndpoint = async () => {{
        const start = performance.now();
        const response = await fetch('{endpoint}');
        const end = performance.now();
        return {{
          url: '{endpoint}',
          status: response.status,
          responseTime: end - start,
          size: response.headers.get('content-length')
        }};
      }};
      return testEndpoint();
    `)
```

**Validation:**
- Homepage loads within 3 seconds under normal conditions
- API endpoints respond within 500ms for 95% of requests
- No 5xx errors during concurrent user simulation
- Application remains responsive with 10+ concurrent users
- Database queries complete within acceptable timeframes
- Memory usage remains stable during load testing

### 8.2 Performance Benchmarking Procedures
**Task:** Establish and validate performance benchmarks for critical application metrics.

**AI Agent Prompt:** Measure and document key performance indicators including page load times, API response times, database query performance, and resource utilization. Compare current performance against established benchmarks and identify any regressions or areas for optimization.

**Tool Calls:**
```bash
# Create performance benchmarks table
mcp__sqlite__create_table(query="CREATE TABLE IF NOT EXISTS performance_benchmarks (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  metric_name TEXT,
  metric_value REAL,
  metric_unit TEXT,
  threshold_value REAL,
  status TEXT,
  page_url TEXT
)")

# Test First Contentful Paint (FCP) and Largest Contentful Paint (LCP)
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
mcp__puppeteer__puppeteer_evaluate(script=`
  const getPerformanceMetrics = () => {
    return new Promise((resolve) => {
      new PerformanceObserver((list) => {
        const entries = list.getEntries();
        const metrics = {};
        
        entries.forEach((entry) => {
          if (entry.entryType === 'paint') {
            metrics[entry.name] = entry.startTime;
          }
          if (entry.entryType === 'largest-contentful-paint') {
            metrics['largest-contentful-paint'] = entry.startTime;
          }
        });
        
        // Also get navigation timing
        const navigation = performance.getEntriesByType('navigation')[0];
        metrics['dom-content-loaded'] = navigation.domContentLoadedEventEnd - navigation.domContentLoadedEventStart;
        metrics['full-load-time'] = navigation.loadEventEnd - navigation.loadEventStart;
        
        resolve(metrics);
      }).observe({entryTypes: ['paint', 'largest-contentful-paint']});
      
      setTimeout(() => resolve({}), 5000); // Timeout after 5 seconds
    });
  };
  
  return getPerformanceMetrics();
`)

# Test API endpoint performance benchmarks
mcp__puppeteer__puppeteer_evaluate(script=`
  const apiPerformanceTest = async () => {
    const endpoints = ['/api/health', '/api/users', '/api/dashboard', '/api/data'];
    const results = [];
    
    for (const endpoint of endpoints) {
      const start = performance.now();
      try {
        const response = await fetch(endpoint);
        const end = performance.now();
        results.push({
          endpoint,
          responseTime: end - start,
          status: response.status,
          size: parseInt(response.headers.get('content-length') || 0)
        });
      } catch (error) {
        results.push({
          endpoint,
          error: error.message,
          responseTime: -1
        });
      }
    }
    return results;
  };
  
  return apiPerformanceTest();
`)

# Store benchmark results
# Note: This would need to be integrated with the actual results from the JavaScript execution
mcp__sqlite__write_query(query="INSERT INTO performance_benchmarks (metric_name, metric_value, metric_unit, threshold_value, status, page_url) 
  VALUES ('first-contentful-paint', 1500, 'ms', 2000, 'PASS', 'https://your-app-domain.com')")
```

**Validation:**
- First Contentful Paint (FCP) < 2 seconds
- Largest Contentful Paint (LCP) < 2.5 seconds
- Time to Interactive (TTI) < 3 seconds
- API response times < 200ms for critical endpoints
- Database query times < 100ms for simple queries
- Bundle size optimized (JavaScript < 200KB gzipped)
- Images optimized with appropriate formats and compression

### 8.3 Caching Strategy Verification
**Task:** Verify that caching mechanisms are properly implemented and functioning as expected.

**AI Agent Prompt:** Test the application's caching strategies including browser caching, CDN caching, API response caching, and database query caching. Verify cache headers are properly set, cache invalidation works correctly, and performance improvements are measurable.

**Tool Calls:**
```bash
# Test browser caching headers
mcp__fetch__fetch(url="https://your-app-domain.com", raw=true)
mcp__fetch__fetch(url="https://your-app-domain.com/static/js/main.js", raw=true)
mcp__fetch__fetch(url="https://your-app-domain.com/static/css/main.css", raw=true)

# Test API response caching
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
mcp__puppeteer__puppeteer_evaluate(script=`
  const testApiCaching = async () => {
    const endpoint = '/api/data';
    const results = [];
    
    // First request (should be fresh)
    const start1 = performance.now();
    const response1 = await fetch(endpoint);
    const end1 = performance.now();
    results.push({
      request: 'first',
      responseTime: end1 - start1,
      cacheStatus: response1.headers.get('x-cache') || 'unknown',
      etag: response1.headers.get('etag')
    });
    
    // Second request (should be cached)
    const start2 = performance.now();
    const response2 = await fetch(endpoint);
    const end2 = performance.now();
    results.push({
      request: 'second',
      responseTime: end2 - start2,
      cacheStatus: response2.headers.get('x-cache') || 'unknown',
      etag: response2.headers.get('etag')
    });
    
    return results;
  };
  
  return testApiCaching();
`)

# Test cache invalidation
mcp__puppeteer__puppeteer_evaluate(script=`
  // Simulate cache-busting scenario
  const testCacheInvalidation = async () => {
    // Make request with cache-busting parameter
    const response = await fetch('/api/data?v=' + Date.now());
    return {
      status: response.status,
      cacheControl: response.headers.get('cache-control'),
      pragma: response.headers.get('pragma')
    };
  };
  
  return testCacheInvalidation();
`)
```

**Validation:**
- Static assets have appropriate cache headers (1 year for versioned files)
- API responses have proper cache-control headers
- ETags are present for cacheable resources
- Cache invalidation works when content changes
- CDN cache hit rates are above 80% for static content
- Performance improvement measurable with caching enabled

### 8.4 Resource Utilization Monitoring
**Task:** Monitor and optimize server resource utilization including CPU, memory, disk, and network usage.

**AI Agent Prompt:** Collect and analyze server resource utilization metrics during application usage. Monitor for memory leaks, CPU spikes, disk space usage, and network bottlenecks. Store metrics in database for trending analysis and establish alerts for resource thresholds.

**Tool Calls:**
```bash
# Create resource monitoring table
mcp__sqlite__create_table(query="CREATE TABLE IF NOT EXISTS resource_metrics (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  metric_type TEXT,
  metric_name TEXT,
  metric_value REAL,
  unit TEXT,
  threshold_warning REAL,
  threshold_critical REAL,
  status TEXT
)")

# Monitor client-side resource usage during application use
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
mcp__puppeteer__puppeteer_evaluate(script=`
  const monitorResources = () => {
    const metrics = {};
    
    // Memory usage (if available)
    if (performance.memory) {
      metrics.memoryUsed = performance.memory.usedJSHeapSize;
      metrics.memoryTotal = performance.memory.totalJSHeapSize;
      metrics.memoryLimit = performance.memory.jsHeapSizeLimit;
    }
    
    // Network resource timing
    const resources = performance.getEntriesByType('resource');
    metrics.totalResources = resources.length;
    metrics.totalTransferSize = resources.reduce((sum, r) => sum + (r.transferSize || 0), 0);
    metrics.totalEncodedSize = resources.reduce((sum, r) => sum + (r.encodedBodySize || 0), 0);
    
    // Document timing
    const navigation = performance.getEntriesByType('navigation')[0];
    if (navigation) {
      metrics.domSize = document.querySelectorAll('*').length;
      metrics.documentCompleteTime = navigation.domContentLoadedEventEnd;
    }
    
    return metrics;
  };
  
  return monitorResources();
`)

# Test resource usage under load
mcp__puppeteer__puppeteer_evaluate(script=`
  const stressTestResources = async () => {
    const results = [];
    
    // Create multiple concurrent requests to stress test
    const promises = [];
    for (let i = 0; i < 20; i++) {
      promises.push(
        fetch('/api/data').then(r => ({
          status: r.status,
          size: r.headers.get('content-length'),
          timing: performance.now()
        }))
      );
    }
    
    const responses = await Promise.all(promises);
    
    // Check memory usage after stress test
    const finalMemory = performance.memory ? {
      used: performance.memory.usedJSHeapSize,
      total: performance.memory.totalJSHeapSize
    } : null;
    
    return {
      responses: responses.length,
      errors: responses.filter(r => r.status >= 400).length,
      finalMemory
    };
  };
  
  return stressTestResources();
`)

# Store resource metrics
mcp__sqlite__write_query(query="INSERT INTO resource_metrics (metric_type, metric_name, metric_value, unit, threshold_warning, threshold_critical, status) 
  VALUES ('memory', 'heap_used', 50.0, 'MB', 100.0, 200.0, 'OK')")

mcp__sqlite__write_query(query="INSERT INTO resource_metrics (metric_type, metric_name, metric_value, unit, threshold_warning, threshold_critical, status) 
  VALUES ('network', 'total_transfer_size', 1.5, 'MB', 5.0, 10.0, 'OK')")
```

**Validation:**
- Memory usage remains stable over time (no memory leaks)
- CPU utilization stays below 80% during normal operations
- Disk space usage is monitored and has adequate free space
- Network transfer sizes are optimized
- Resource usage scales appropriately with concurrent users
- No resource exhaustion during peak usage periods

### 8.5 Performance Optimization Validation
**Task:** Validate that performance optimizations are properly implemented and effective.

**AI Agent Prompt:** Test and verify performance optimizations including code splitting, lazy loading, image optimization, minification, compression, and other performance enhancements. Measure the impact of optimizations and ensure they provide measurable performance improvements.

**Tool Calls:**
```bash
# Test code splitting and lazy loading
mcp__puppeteer__puppeteer_navigate(url="https://your-app-domain.com")
mcp__puppeteer__puppeteer_evaluate(script=`
  const testCodeSplitting = () => {
    const scripts = Array.from(document.querySelectorAll('script[src]'));
    const stylesheets = Array.from(document.querySelectorAll('link[rel="stylesheet"]'));
    
    return {
      initialScripts: scripts.length,
      initialStylesheets: stylesheets.length,
      scriptSizes: scripts.map(s => s.src),
      stylesheetSizes: stylesheets.map(s => s.href)
    };
  };
  
  return testCodeSplitting();
`)

# Test lazy loading by navigating to different sections
mcp__puppeteer__puppeteer_click(selector="[data-testid='dashboard-link']")
mcp__puppeteer__puppeteer_evaluate(script=`
  // Check if new scripts/styles were loaded dynamically
  const newScripts = Array.from(document.querySelectorAll('script[src]'));
  const newStylesheets = Array.from(document.querySelectorAll('link[rel="stylesheet"]'));
  
  return {
    totalScriptsAfterNavigation: newScripts.length,
    totalStylesheetsAfterNavigation: newStylesheets.length,
    dynamicallyLoaded: performance.getEntriesByType('resource')
      .filter(r => r.initiatorType === 'script' || r.initiatorType === 'css')
      .length
  };
`)

# Test image optimization
mcp__puppeteer__puppeteer_evaluate(script=`
  const testImageOptimization = () => {
    const images = Array.from(document.querySelectorAll('img'));
    return images.map(img => ({
      src: img.src,
      format: img.src.split('.').pop(),
      hasLazyLoading: img.loading === 'lazy',
      hasSrcset: !!img.srcset,
      naturalWidth: img.naturalWidth,
      naturalHeight: img.naturalHeight,
      displayWidth: img.width,
      displayHeight: img.height
    }));
  };
  
  return testImageOptimization();
`)

# Test compression and minification
mcp__fetch__fetch(url="https://your-app-domain.com/static/js/main.js", raw=true)
mcp__fetch__fetch(url="https://your-app-domain.com/static/css/main.css", raw=true)

# Performance comparison test
mcp__sqlite__create_table(query="CREATE TABLE IF NOT EXISTS optimization_results (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  optimization_type TEXT,
  before_value REAL,
  after_value REAL,
  improvement_percent REAL,
  metric_unit TEXT
)")
```

**Validation:**
- JavaScript bundles are properly minified and compressed
- Code splitting reduces initial bundle size by >30%
- Lazy loading defers non-critical resources
- Images use modern formats (WebP, AVIF where supported)
- Gzip/Brotli compression enabled for text resources
- Critical CSS is inlined for above-the-fold content
- Performance scores improved measurably (Lighthouse scores >90)

## 9. Documentation & Communication

### 9.1 Automated Documentation Updates
**Task:** Update deployment documentation and API specs automatically
**AI Agent Prompt:** "Update the deployment documentation by analyzing the current project structure, deployment configuration, and environment setup. Create or update comprehensive documentation that includes setup instructions, configuration details, and troubleshooting guides. Ensure all API endpoints are documented with current schemas and examples."
**Tool Calls:**
- `Glob` to find existing documentation files: `**/*.md`, `**/docs/**/*.md`
- `Read` to examine current documentation content
- `Edit` or `Write` to update deployment guides and create new documentation files
- `mcp__firecrawl__firecrawl_scrape` to extract API documentation from running services
- `Write` to create `docs/deployment-guide.md`, `docs/api-documentation.md`, `docs/troubleshooting.md`
**Validation:** Verify all deployment steps are documented in markdown files, API documentation matches current implementation, and troubleshooting guides are complete and accessible in the repository

### 9.2 API Documentation Validation
**Task:** Validate API documentation accuracy against live endpoints
**AI Agent Prompt:** "Validate that all API documentation matches the current implementation by scraping live API endpoints, comparing schemas, and updating any discrepancies. Generate OpenAPI/Swagger documentation if not present."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_scrape` to extract API endpoint documentation
- `mcp__puppeteer__puppeteer_navigate` to access API documentation interfaces
- `mcp__puppeteer__puppeteer_screenshot` to capture API documentation states
- `Read` to examine existing API documentation files
- `Edit` or `Write` to update `docs/api-endpoints.md`, `docs/api-schemas.md`, `openapi.yaml`
- `Write` to create comprehensive API documentation with examples and schemas
**Validation:** All API endpoints documented in markdown files, request/response schemas accurate, authentication methods clearly explained in repository documentation

### 9.3 Rollback Procedures Documentation
**Task:** Document comprehensive rollback procedures for all deployment scenarios
**AI Agent Prompt:** "Create detailed rollback procedures covering database migrations, application versions, configuration changes, and CapRover-specific rollback steps. Include step-by-step instructions, rollback time estimates, and risk assessments for each rollback scenario."
**Tool Calls:**
- `Write` to create `docs/rollback-procedures.md` with comprehensive rollback documentation
- `Edit` to update existing rollback documentation with current procedures
- `Write` to create `docs/emergency-procedures.md` with step-by-step emergency response
- `Bash` to test rollback commands and document outputs
- `Write` to create rollback checklists and validation procedures in markdown format
**Validation:** Rollback procedures documented and tested, time estimates validated, emergency contact information included in repository documentation

### 9.4 LLMs.txt Generation and AI Guidelines
**Task:** Generate LLMs.txt file for AI model interaction guidelines
**AI Agent Prompt:** "Generate a comprehensive LLMs.txt file that defines how AI models should interact with this application, including allowed endpoints, rate limits, usage guidelines, and prohibited actions."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_generate_llmstxt` to create standardized LLMs.txt
- `mcp__firecrawl__firecrawl_map` to discover all application URLs
- `Edit` to customize LLMs.txt with specific application guidelines
**Validation:** LLMs.txt file accessible at domain root, guidelines comprehensive, rate limits specified

### 9.5 Communication Plan Setup
**Task:** Prepare automated communication for deployment notifications
**AI Agent Prompt:** "Set up automated communication channels for deployment notifications including success/failure alerts, rollback notifications, and stakeholder updates. Create templates for different deployment scenarios."
**Tool Calls:**
- `Write` to create `docs/communication-templates.md` with notification templates
- `Write` to create `docs/stakeholder-contacts.md` with contact information and escalation procedures
- `Bash` to configure webhook notifications
- `Edit` to update existing communication documentation with current procedures
- `Write` to create deployment notification scripts and templates in the repository
**Validation:** All stakeholders identified and documented, notification channels tested, escalation procedures documented in repository files

## 10. Final Validation

### 10.1 End-to-End Testing with Browser Automation
**Task:** Perform comprehensive end-to-end testing using automated browser testing
**AI Agent Prompt:** "Execute complete end-to-end testing scenarios using browser automation to validate all critical user journeys, form submissions, authentication flows, and payment processes. Test across different screen sizes and browsers if applicable."
**Tool Calls:**
- `mcp__puppeteer__puppeteer_navigate` to access staging environment
- `mcp__puppeteer__puppeteer_click` to interact with UI elements
- `mcp__puppeteer__puppeteer_fill` to test form submissions
- `mcp__puppeteer__puppeteer_screenshot` to capture test evidence
- `mcp__puppeteer__puppeteer_evaluate` to run custom JavaScript validations
**Validation:** All critical user flows completed successfully, no JavaScript errors, performance meets benchmarks

### 10.2 Staging Environment Comprehensive Validation
**Task:** Validate staging environment configuration and content completeness
**AI Agent Prompt:** "Perform comprehensive validation of the staging environment by crawling all pages, testing all functionality, and comparing configuration against production requirements. Ensure staging mirrors production setup exactly."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_crawl` to comprehensively test staging environment
- `mcp__firecrawl__firecrawl_map` to discover all available URLs
- `mcp__firecrawl__firecrawl_scrape` to validate individual page functionality
- `mcp__puppeteer__puppeteer_navigate` to test interactive elements
**Validation:** All pages accessible, no broken links, environment variables correctly set, database connections working

### 10.3 CapRover Webhook Integration Testing
**Task:** Test CapRover webhook integration and auto-deployment functionality
**AI Agent Prompt:** "Test CapRover webhook integration by triggering deployments, monitoring webhook responses, and validating auto-deployment functionality. Ensure webhooks properly handle different git events and deployment scenarios."
**Tool Calls:**
- `Bash` to trigger webhook calls and monitor responses
- `mcp__fetch__fetch` to test webhook endpoints directly
- `mcp__puppeteer__puppeteer_navigate` to access CapRover dashboard
- `mcp__puppeteer__puppeteer_screenshot` to capture deployment status
**Validation:** Webhooks respond correctly, auto-deployment triggers properly, deployment logs accessible

### 10.4 Rollback Functionality Verification
**Task:** Test all rollback mechanisms and recovery procedures
**AI Agent Prompt:** "Execute rollback testing by performing actual rollbacks in staging environment, testing database rollback procedures, and validating one-click rollback functionality in CapRover. Document rollback times and validate data integrity."
**Tool Calls:**
- `Bash` to execute rollback commands
- `mcp__puppeteer__puppeteer_navigate` to test CapRover rollback interface
- `mcp__puppeteer__puppeteer_click` to trigger one-click rollbacks
- `mcp__sqlite__read_query` to validate database state after rollbacks
**Validation:** Rollback completes within expected timeframe, data integrity maintained, application functional after rollback

### 10.5 Production Readiness Final Check
**Task:** Perform final production readiness validation across all systems
**AI Agent Prompt:** "Conduct final production readiness check by validating SSL certificates, DNS configuration, monitoring systems, backup procedures, and performance benchmarks. Ensure all production requirements are met and documented."
**Tool Calls:**
- `mcp__firecrawl__firecrawl_scrape` to validate SSL certificate status
- `Bash` to test DNS resolution and connectivity
- `mcp__puppeteer__puppeteer_navigate` to validate production URLs
- `mcp__firecrawl__firecrawl_search` to verify public visibility settings
- `mcp__sqlite__read_query` to validate database performance
**Validation:** SSL certificates valid, DNS resolving correctly, monitoring active, backups configured, performance benchmarks met

---

## CapRover Deployment Setup Instructions

### Prerequisites Setup
1. **Install CapRover CLI locally:**
   ```bash
   npm install -g caprover
   ```

2. **Login to CapRover server:**
   ```bash
   caprover login
   ```

### App Creation & Configuration

#### 1. Create New App (Dashboard Page)
- Navigate to Apps → Create New App
- **App Name:** Enter your application name (e.g., `my-amazing-app`)
- **Has Persistent Data:** Check only if your app needs persistent storage (databases, file uploads, etc.)
- Click "Create New App"

#### 2. HTTP Settings Configuration
- Navigate to your app → HTTP Settings tab
- **Default Domain:** `appname.your-root-domain.com` (automatically assigned)
- **Custom Domains:** Add production domains (e.g., `www.myapp.com`, `myapp.com`)
  - Enter domain in text field
  - Click "Connect New Domain"
  - **Enable HTTPS** for each domain (recommended)
  - **Force HTTPS** to redirect HTTP traffic to HTTPS
- **Container HTTP Port:** Set if your app runs on a port other than 80 (e.g., 3000, 8080, 5000)
- **Websocket Support:** Enable if your app uses websockets
- **Do not expose as web app:** Check only for non-web services (databases, background workers)

#### 3. App Configuration (App Configs Page)
- Navigate to your app → App Configs tab

**Environment Variables:**
- Add all required environment variables your app needs
- Common examples:
  - `NODE_ENV=production`
  - `PORT=80` (or your app's port)
  - Database connection strings
  - API keys and secrets
  - Third-party service credentials

**Port Mapping:**
- Only add if you need external access to specific ports
- Common use case: Database connections from external tools
- Format: Host Port → Container Port

**Persistent Directories:**
- Only configure if "Has Persistent Data" was enabled
- **Path in App:** Container directory (e.g., `/app/data`, `/var/lib/mysql`)
- **Label:** Volume name (e.g., `app-data`, `mysql-data`)
- **Or specify host path:** Absolute path on server

**Instance Count:**
- Default: 1
- Increase only for stateless apps that can handle multiple instances
- Consider server resources (RAM/CPU)

**Service Tags:**
- Optional: Add tags for organizing apps (e.g., `production`, `api`, `frontend`)

#### 4. Initial Deployment Setup

**Option A: CLI Deployment (Recommended for Claude Code)**
1. **First-time setup (manual):**
   ```bash
   caprover deploy
   ```
   - Select CapRover server
   - Choose app name
   - Select git branch
   - This creates deployment configuration

2. **Subsequent deployments (Claude Code compatible):**
   ```bash
   caprover deploy -d
   ```
   - Uses previously saved configuration
   - Non-interactive, perfect for Claude Code

**Option B: Git Auto-Deploy (Alternative)**
- Navigate to app → Deployment tab
- Configure repository settings:
  - **Repository:** `github.com/username/repo` (no https:// prefix)
  - **Branch:** `main` or `master`
  - **Username:** GitHub username
  - **Password:** Personal access token (not actual password)
- Save configuration
- Copy webhook URL
- Add webhook to GitHub repository settings

### Required Files in Your Project

#### Captain Definition File
Create `captain-definition` in your project root:

**Simple Node.js example:**
```json
{
  "schemaVersion": 2,
  "templateId": "node/18"
}
```

**Custom Dockerfile example:**
```json
{
  "schemaVersion": 2,
  "dockerfilePath": "./Dockerfile"
}
```

**Advanced example with custom build:**
```json
{
  "schemaVersion": 2,
  "dockerfileLines": [
    "FROM node:18-alpine",
    "WORKDIR /app",
    "COPY package*.json ./",
    "RUN npm ci --only=production",
    "COPY . .",
    "EXPOSE 80",
    "CMD [\"npm\", \"start\"]"
  ]
}
```

### Deployment Workflow for Claude Code

1. **Initial Setup (One-time, manual):**
   - Create app via CapRover dashboard
   - Configure HTTP settings and environment variables
   - Run first deployment: `caprover deploy` (interactive)

2. **Ongoing Deployments (Claude Code):**
   - Ensure all changes are committed to git
   - Run: `caprover deploy -d` (non-interactive)
   - Monitor deployment logs for issues

3. **Troubleshooting:**
   - Check app logs: `docker service logs srv-captain--your-app-name --follow`
   - Restart app: Click "Save & Restart" in CapRover dashboard
   - Rollback: Use one-click rollback in Deployment tab

### Post-Deployment Verification
- [ ] App accessible via assigned CapRover domain
- [ ] Custom domains working (if configured)
- [ ] HTTPS certificates issued and working
- [ ] Environment variables properly set
- [ ] Application logs show no errors
- [ ] Database connections working (if applicable)
- [ ] External integrations functioning