Most common end-user errors
Here are the most common end-user errors seen when accessing a microservices application running on AWS EKS. These errors appear on the user’s browser or client when something in the path (DNS → Load Balancer → Ingress → Service → Pod → DB → External API) is broken.
I’ve grouped them by source (DNS, Load Balancer, Ingress, Kubernetes service, Pod/app, database, network, authentication, rate limiting).

✅ 1. DNS / Domain-related Errors
These appear before traffic even reaches AWS:
❌ 1. DNS_PROBE_FINISHED_NXDOMAIN
Domain doesn’t exist or DNS misconfigured
Route 53 record missing or wrong


❌ 2. DNS_PROBE_STARTED / DNS timeout
DNS not resolving
Propagation delay
Route 53 health check routing failure



✅ 2. Load Balancer / Network / Ingress Errors
❌ 3. 503 Service Unavailable
ALB has no healthy targets
Ingress controller is failing
Kubernetes pods not passing readiness probes
Service has 0 endpoints


❌ 4. 502 Bad Gateway
ALB or Ingress cannot forward to backend
Pod crashes or app not listening on the expected port
Wrong target group health checks


❌ 5. 504 Gateway Timeout
Pod processing takes too long
Database/API is slow
NLB/ALB timeout exceeded
Ingress timeout misconfigured


❌ 6. Connection refused
Service or pod is not listening
Container port mismatch
NetworkPolicy blocking traffic


❌ 7. SSL/TLS errors
“Your connection is not private”
“Certificate expired / invalid”
Wrong ACM certificate on ALB
Ingress TLS misconfigured



✅ 3. Application / Pod / Microservice Errors
❌ 8. 500 Internal Server Error
Unhandled exception in app
Microservice logic error
Pod resource starvation (OOMKilled, CPU throttling)


❌ 9. 404 Not Found
Wrong Ingress routing path
Backend route not implemented
ALB rule mismatch


❌ 10. 403 Forbidden
Auth service / API gateway blocking
IAM authorizer failure
Incorrect JWT token



✅ 4. Database / Cache Errors
❌ 11. 503 / 504 Errors due to DB unavailable
RDS failover
DB reached max connections
NetworkPolicy or SG blocking DB port


❌ 12. "Database connection failed"
Secrets wrong
Wrong DB hostname / port
DNS resolution inside cluster failing


❌ 13. 502/504 Errors when Redis/ElastiCache unreachable
Cache endpoint changed
SG rules misconfigured



✅ 5. Timeout & Performance-related Errors
❌ 14. 408 Request Timeout
App taking too long to respond
Ingress idle timeout hit


❌ 15. 429 Too Many Requests
Rate limiting by WAF
API throttling
HPA not scaling enough pods



✅ 6. Security & WAF-related Errors
❌ 16. 403 Forbidden (WAF Blocked)
WAF detects SQL injection, XSS, or bot traffic
IP reputation blocked
Country-based blocking


❌ 17. Captcha or Challenge
WAF Bot Control challenge page displayed



✅ 7. Authentication / Authorization Errors
❌ 18. Unauthorized (401)
Token expired
Login session invalid
Auth microservice failure


❌ 19. Forbidden (403)
User is authenticated but not authorized


RBAC / API permission issue



✅ 8. Service Mesh / MTLS Errors (if using Istio / AppMesh)
❌ 20. 503 UF / UO / NR Errors
Istio-sidecar errors:
UF: Upstream connection failure
NR: No route configured
UO: Upstream overflow


❌ 21. TLS handshake error
mTLS misconfigured
Certificates expired



🔥 Most common EKS-specific end-user errors (short list)
If an interviewer asks:
503 Service Unavailable → No healthy pods/endpoints
502 Bad Gateway → Ingress/ALB cannot reach pod
504 Gateway Timeout → Slow backend/DB
404 Not Found → Wrong Ingress rules
500 Internal Server Error → App code failure
403 / 401 → Auth failures
DNS NXDOMAIN → Wrong DNS or Route 53 misconfig
SSL Certificate Errors → ACM/Ingress TLS issues


