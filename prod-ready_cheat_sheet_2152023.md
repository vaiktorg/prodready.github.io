---data---
title: prod_ready_cheat_sheet
author: VKTRG
date: 15 Feb 2026
file: prod_ready_cheat_sheet.md
path: ./
---data---


# Production-Ready Feature Checklist

A comprehensive guide for shipping features to production with confidence.

---

## 1. Functionality & Requirements

### Core Functionality
- [ ] Feature works as specified in requirements/tickets
- [ ] All acceptance criteria met
- [ ] Happy path flows end-to-end
- [ ] Feature behaves correctly across all supported platforms/browsers (if applicable)

### Edge Cases & Boundary Conditions
- [ ] **Empty/null/undefined inputs** - What happens with missing data?
- [ ] **Zero values** - Does `0`, `0.0`, empty array `[]`, empty string `""` break anything?
- [ ] **Negative numbers** - If numeric input, are negatives handled?
- [ ] **Very large values** - Max integer, long strings, huge arrays
- [ ] **Very small values** - Decimal precision, rounding issues
- [ ] **Special characters** - Unicode, emojis, SQL/HTML characters in text
- [ ] **Whitespace** - Leading/trailing spaces, tabs, newlines
- [ ] **Duplicate data** - Same item added twice, concurrent submissions
- [ ] **Out-of-order operations** - What if steps happen in wrong sequence?
- [ ] **Stale data** - Cached or outdated information
- [ ] **Concurrent operations** - Multiple users/processes modifying same data
- [ ] **Timezone handling** - If dates/times involved, tested across timezones?
- [ ] **Deleted/archived dependencies** - References to deleted users, posts, etc.

### Error States
- [ ] Network failures handled gracefully
- [ ] Timeout scenarios covered
- [ ] Third-party service failures don't break the app
- [ ] Database connection issues handled
- [ ] Partial failures handled (e.g., batch operations)
- [ ] User sees helpful error messages, not stack traces

### User Feedback
- [ ] Loading states for async operations
- [ ] Success confirmations
- [ ] Error messages are actionable ("Try again" vs "Contact support")
- [ ] Progress indicators for long operations
- [ ] Validation errors clearly shown

---

## 2. Code Quality

### Readability & Maintainability
- [ ] Code is self-documenting with clear variable/function names
- [ ] Complex logic has explanatory comments
- [ ] Functions are single-purpose and reasonably sized (<50 lines ideal)
- [ ] No "magic numbers" - use named constants
- [ ] DRY principle followed (no unnecessary duplication)
- [ ] Consistent formatting (linter passes)

### Configuration & Environment
- [ ] No hardcoded values (URLs, API keys, thresholds)
- [ ] Environment variables used for config
- [ ] Different configs for dev/staging/prod
- [ ] Secrets never committed to repo
- [ ] Feature flags used if needed for gradual rollout

### Error Handling
- [ ] All external calls wrapped in try/catch (or equivalent)
- [ ] Errors logged with sufficient context
- [ ] Errors don't expose sensitive information
- [ ] Fallback behaviors defined
- [ ] Errors propagate or are handled at appropriate level

### Performance Considerations
- [ ] No obvious O(n²) or worse algorithms where avoidable
- [ ] Database queries optimized (see Performance section)
- [ ] Large files/data streamed not loaded into memory
- [ ] Infinite loops impossible
- [ ] Resource cleanup (close connections, files, streams)

### Dependencies
- [ ] New dependencies necessary and vetted (license, maintenance, size)
- [ ] Dependencies pinned to specific versions
- [ ] No known vulnerabilities (security scan passed)

---

## 3. Testing

### Unit Tests
- [ ] **Happy path** covered
- [ ] **Edge cases** tested (null, empty, boundary values)
- [ ] **Error conditions** tested (exceptions, invalid input)
- [ ] **Mocking** used appropriately for external dependencies
- [ ] Test names describe what they test
- [ ] Tests are deterministic (no flaky tests)
- [ ] Fast execution (<5 seconds for unit suite)
- [ ] Code coverage meets team standards (typically 70-80%+)

### Integration Tests
- [ ] Feature works end-to-end in test environment
- [ ] Database transactions work correctly
- [ ] External API integrations tested (or mocked appropriately)
- [ ] Authentication/authorization flows work
- [ ] Multi-step workflows complete successfully

### Manual Testing
- [ ] Tested in browser/app on your machine
- [ ] Tested in staging/pre-prod environment
- [ ] Tested on different devices/screen sizes (if UI)
- [ ] Tested with real-ish data (not just `test@test.com`)
- [ ] Tested while logged in as different user roles
- [ ] Tested both with and without feature flags enabled

### Regression Testing
- [ ] Existing tests still pass
- [ ] Related features still work
- [ ] No unexpected side effects in other parts of the system

### Common Test Scenarios (if applicable)
- [ ] **Empty state** - First time user, no data yet
- [ ] **Pagination** - First page, last page, page boundaries
- [ ] **Permissions** - Unauthorized user can't access
- [ ] **Rate limiting** - Excessive requests handled
- [ ] **Idempotency** - Same request twice produces same result

---

## 4. Security

### Input Validation
- [ ] All user input validated (type, format, range)
- [ ] Server-side validation (never trust client)
- [ ] Whitelist approach preferred over blacklist
- [ ] File uploads: type, size, content validation
- [ ] Max length checks on text inputs

### Injection Prevention
- [ ] **SQL Injection** - Parameterized queries/ORMs used, no string concatenation
- [ ] **XSS** - User input escaped/sanitized before display
- [ ] **Command Injection** - No unsanitized input to shell commands
- [ ] **Path Traversal** - File paths validated, no `../` attacks
- [ ] **LDAP/XML/etc Injection** - Context-specific escaping

### Authentication & Authorization
- [ ] Authentication required where needed
- [ ] Authorization checks on all endpoints (not just UI)
- [ ] User can only access their own resources
- [ ] Role-based access control works correctly
- [ ] Admin-only features properly gated
- [ ] Token/session expiration handled

### Data Protection
- [ ] Sensitive data encrypted at rest (if applicable)
- [ ] Sensitive data encrypted in transit (HTTPS)
- [ ] PII/PHI handled per compliance requirements (GDPR, HIPAA, etc.)
- [ ] Passwords hashed with strong algorithm (bcrypt, argon2)
- [ ] API keys/tokens not logged or exposed
- [ ] No sensitive data in URLs (use POST body)

### Common Vulnerabilities (OWASP Top 10)
- [ ] **CSRF** - Tokens used for state-changing operations
- [ ] **Open Redirects** - Redirect URLs validated
- [ ] **SSRF** - User-provided URLs validated
- [ ] **Mass Assignment** - Only allowed fields can be updated
- [ ] **Insecure Deserialization** - Don't deserialize untrusted data
- [ ] **Dependency vulnerabilities** - Security scan passed

### API Security (if applicable)
- [ ] Rate limiting implemented
- [ ] API keys/tokens validated
- [ ] CORS configured correctly
- [ ] GraphQL: depth/complexity limiting (if applicable)

---

## 5. Observability

### Logging
- [ ] Key operations logged (create, update, delete)
- [ ] Errors logged with stack traces
- [ ] Logs include context (user ID, request ID, timestamp)
- [ ] Log levels used appropriately (DEBUG, INFO, WARN, ERROR)
- [ ] No sensitive data in logs (passwords, tokens, PII)
- [ ] Structured logging used (JSON format preferred)
- [ ] Request IDs for tracing requests across services

### Metrics & Monitoring
- [ ] Key performance indicators tracked (latency, throughput)
- [ ] Error rates monitored
- [ ] Business metrics tracked if applicable (signups, conversions)
- [ ] Dashboards created for key metrics
- [ ] Database query performance tracked

### Alerting
- [ ] Alerts configured for critical errors
- [ ] Alert thresholds set appropriately
- [ ] On-call team knows how to respond to alerts
- [ ] Alert fatigue avoided (not too noisy)

### Error Tracking
- [ ] Errors sent to tracking service (Sentry, Rollbar, etc.)
- [ ] Error grouping/deduplication configured
- [ ] Source maps uploaded (if JavaScript/TypeScript)
- [ ] Context attached to errors (user, environment, etc.)

### Debugging Capabilities
- [ ] Can reproduce issues from logs alone
- [ ] Can trace request through system
- [ ] Can identify which deployment introduced issue
- [ ] Have access to production logs (with proper security)

---

## 6. Performance

### Database Optimization
- [ ] **Indexes** created for frequently queried columns
- [ ] **N+1 queries** eliminated (use eager loading, joins)
- [ ] **Query complexity** - No cartesian products, efficient joins
- [ ] **Pagination** implemented for large result sets
- [ ] **Connection pooling** configured
- [ ] **Transactions** scoped appropriately (not too large)
- [ ] **Batch operations** used where applicable
- [ ] **EXPLAIN** analysis done on complex queries

### Caching Strategy
- [ ] Cache frequently accessed, infrequently changed data
- [ ] Cache invalidation strategy defined
- [ ] Cache TTL set appropriately
- [ ] Cache hit/miss rates monitored
- [ ] Consider: Redis, Memcached, CDN, HTTP caching

### API Performance
- [ ] Response times under acceptable threshold (<200ms ideal, <1s acceptable)
- [ ] Timeouts configured for external calls
- [ ] Retry logic with exponential backoff
- [ ] Circuit breakers for failing dependencies
- [ ] Compression enabled (gzip/brotli)

### Frontend Performance (if applicable)
- [ ] Bundle size reasonable (<200KB gzipped initial load)
- [ ] Images optimized and lazy-loaded
- [ ] Critical CSS inlined
- [ ] JavaScript deferred/async where possible
- [ ] Lighthouse score reviewed

### Load Testing
- [ ] **Required if:** Feature handles high traffic, payments, or critical operations
- [ ] Expected load tested (concurrent users, requests/second)
- [ ] Peak load tested (2-3x expected)
- [ ] Resource usage acceptable (CPU, memory, disk)
- [ ] No memory leaks over extended runs
- [ ] Degradation graceful under overload

### Scalability Considerations
- [ ] Feature can scale horizontally if needed
- [ ] No single points of failure
- [ ] Stateless where possible
- [ ] Async processing for slow operations (queues/workers)

---

## 7. Documentation

### Code Documentation
- [ ] Complex algorithms explained
- [ ] Non-obvious decisions documented
- [ ] Public APIs documented (parameters, returns, errors)
- [ ] Architecture diagrams if complex
- [ ] "Why" explained, not just "what"

### API Documentation (if applicable)
- [ ] Endpoints documented (REST/GraphQL schema)
- [ ] Request/response examples provided
- [ ] Authentication requirements specified
- [ ] Rate limits documented
- [ ] Error codes and meanings listed
- [ ] Swagger/OpenAPI spec updated

### Operational Documentation
- [ ] README updated with setup instructions
- [ ] Environment variables documented
- [ ] Configuration options explained
- [ ] Troubleshooting guide (common issues)
- [ ] Runbook for deployments
- [ ] Rollback procedures documented

### User-Facing Documentation (if applicable)
- [ ] User guide or help articles updated
- [ ] Tooltips/in-app guidance added
- [ ] FAQ updated
- [ ] Screenshots/videos created
- [ ] Release notes prepared

### Database Changes
- [ ] Schema changes documented
- [ ] Migration scripts documented
- [ ] Data migration strategy explained
- [ ] Rollback procedures for migrations

---

## 8. Deployment Readiness

### Database Migrations
- [ ] Migrations tested in dev/staging
- [ ] Migrations are reversible (down migrations)
- [ ] Migrations are idempotent (can run multiple times)
- [ ] Large data migrations run in batches
- [ ] Migrations don't lock tables for long periods
- [ ] Backup taken before migration
- [ ] Rollback tested

### Feature Flags
- [ ] **Required if:** Risky change, gradual rollout needed, or A/B testing
- [ ] Flag names descriptive and documented
- [ ] Default state is safe (usually OFF)
- [ ] Can toggle flag without deployment
- [ ] Can target specific users/segments
- [ ] Cleanup plan for removing flag

### Configuration
- [ ] Environment variables set in all environments
- [ ] Config changes don't require code deployment
- [ ] Secrets rotated if needed
- [ ] External service credentials configured

### Dependencies & Infrastructure
- [ ] New services deployed and configured
- [ ] DNS records updated if needed
- [ ] SSL certificates valid
- [ ] Load balancer configured
- [ ] Firewall rules updated
- [ ] Third-party service accounts created

### Deployment Plan
- [ ] Deployment steps documented
- [ ] Deployment window scheduled (if downtime)
- [ ] Stakeholders notified
- [ ] On-call engineer assigned
- [ ] Communication plan (status page, Slack)

### Rollback Plan
- [ ] Can rollback code deployment quickly
- [ ] Can rollback database changes
- [ ] Data loss acceptable or prevented
- [ ] Feature flag can disable feature
- [ ] Rollback tested in staging
- [ ] Criteria for rollback decision defined

### Smoke Tests
- [ ] Post-deployment verification steps defined
- [ ] Automated smoke tests run after deploy
- [ ] Health check endpoints return success
- [ ] Key workflows manually verified

### Monitoring Post-Deploy
- [ ] Error rates monitored for 24-48 hours
- [ ] Performance metrics watched
- [ ] User feedback channels monitored
- [ ] Alerts reviewed
- [ ] Gradual rollout if using feature flags

---

## 9. Compliance & Legal (if applicable)

### Data Privacy
- [ ] GDPR compliance (EU users)
- [ ] CCPA compliance (California users)
- [ ] User consent obtained where required
- [ ] Data retention policies followed
- [ ] Right to deletion implemented
- [ ] Privacy policy updated

### Accessibility (if UI)
- [ ] WCAG 2.1 AA compliance (minimum)
- [ ] Keyboard navigation works
- [ ] Screen reader compatible
- [ ] Color contrast sufficient
- [ ] Alt text on images
- [ ] Focus indicators visible
- [ ] Forms properly labeled

### Industry-Specific
- [ ] **HIPAA** - If handling healthcare data
- [ ] **PCI DSS** - If handling payment cards
- [ ] **SOC 2** - If enterprise SaaS
- [ ] **FERPA** - If handling student data

### Terms of Service
- [ ] Feature within scope of user agreement
- [ ] Doesn't violate platform policies
- [ ] Doesn't introduce new legal risks

---

## 10. Communication & Coordination

### Team Coordination
- [ ] Code review completed and approved
- [ ] Design/product signed off
- [ ] QA/testing team signed off
- [ ] Security review if needed
- [ ] Dependencies on other teams resolved

### Stakeholder Communication
- [ ] Product manager aware of launch timing
- [ ] Support team trained on new feature
- [ ] Marketing team aware if customer-facing
- [ ] Sales team aware if affects customers
- [ ] Legal team consulted if needed

### User Communication
- [ ] In-app announcements if needed
- [ ] Email notifications sent
- [ ] Blog post or changelog published
- [ ] Social media updates coordinated
- [ ] Documentation published

---

## Common Gotchas by Technology

### REST APIs
- [ ] HTTP methods used correctly (GET, POST, PUT, DELETE)
- [ ] Status codes appropriate (200, 201, 400, 401, 404, 500)
- [ ] Pagination for list endpoints
- [ ] Filtering/sorting implemented
- [ ] Versioning strategy (URL, header, or accept-header)
- [ ] HATEOAS considered (links in responses)

### GraphQL APIs
- [ ] No N+1 query problems (DataLoader)
- [ ] Query depth limiting
- [ ] Query complexity limiting
- [ ] Proper error handling in resolvers
- [ ] Subscriptions properly cleaned up

### Microservices
- [ ] Service discovery configured
- [ ] Circuit breakers implemented
- [ ] Distributed tracing enabled
- [ ] Inter-service auth configured
- [ ] Message queue configuration
- [ ] Idempotency keys for operations

### Frontend (React/Vue/Angular)
- [ ] State management clean (Redux/Vuex/etc)
- [ ] Memory leaks prevented (cleanup in unmount)
- [ ] Accessibility props (aria-labels, roles)
- [ ] Loading states for data fetching
- [ ] Error boundaries catch errors
- [ ] SEO meta tags if applicable

### Mobile Apps
- [ ] Works on iOS and Android
- [ ] Different screen sizes handled
- [ ] Offline functionality if needed
- [ ] Push notifications tested
- [ ] App store guidelines followed
- [ ] Permissions requested appropriately

### Background Jobs/Workers
- [ ] Jobs are idempotent
- [ ] Retry logic with backoff
- [ ] Dead letter queue configured
- [ ] Job timeout configured
- [ ] Failure notifications set up
- [ ] Job monitoring dashboard

### Real-time Features (WebSockets/SSE)
- [ ] Connection drop handling
- [ ] Reconnection logic
- [ ] Message ordering guaranteed if needed
- [ ] Scalability (multiple servers)
- [ ] Authentication on connection

---

## Pre-Deploy Checklist (Day Of)

- [ ] All tests passing in CI
- [ ] Staging environment working
- [ ] Code reviewed and merged
- [ ] Database migrations ready
- [ ] Configuration in place
- [ ] Feature flags configured
- [ ] Monitoring/alerts active
- [ ] Team available for support
- [ ] Rollback plan ready
- [ ] Communication sent

---

## Post-Deploy Checklist (First 48 Hours)

- [ ] Smoke tests passed
- [ ] Error rates normal
- [ ] Performance metrics normal
- [ ] No customer complaints
- [ ] Monitoring shows healthy metrics
- [ ] Feature flag enabled (if applicable)
- [ ] Team debriefed on any issues

---

## Notes

**Adjust based on:**
- Team size and maturity
- Feature criticality
- Risk tolerance
- Industry/compliance requirements
- Company stage (startup vs enterprise)

**When to skip items:**
- Small internal tools: lighter testing, documentation
- Prototypes/experiments: skip some production rigor
- Hotfixes: expedited process, but still test

**Red flags that need attention:**
- "We'll document it later" (usually doesn't happen)
- "Tests are flaky but mostly pass" (fix them)
- "Works on my machine" (environment differences)
- "We can rollback if it breaks" (have you tested rollback?)
- "No one will use that edge case" (they will)