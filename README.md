# ProvIDe - Bank-Verified Digital Identity Platform

A modern Fintech platform that enables users to authenticate with digital services using their bank-verified identity, eliminating the need to upload identity documents.

## 📋 Table of Contents

- [Overview](#overview)
- [Team](#team)
- [Problem Statement](#problem-statement)
- [Solution](#solution)
- [Features](#features)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Security & Compliance](#security--compliance)
- [Use Cases](#use-cases)
- [References](#references)

## Overview

ProvIDe is a digital identity platform that leverages banks as trusted identity anchors (Trust Anchors). Instead of requiring users to upload identity documents—a process that introduces friction, security risks, and regulatory complexity—ProvIDe securely delegates authentication to regulated banks via **OpenFinance.ai** and returns cryptographically signed identity assertions that meet enterprise security and compliance standards.

### Key Concept

The platform provides a unified "Authenticate with Bank" button that digital services can integrate, similar to "Sign in with Google" or "Sign in with Facebook," but with higher trust levels and regulatory compliance suitable for financial services.

## Team

**Students:**
- Amit Zamir (322833021)
- Noa Almakais (211632302)
- May Gorvitz (322438177)
- Hai Tal (311561245)

**Supervisor:** Ari Ben Ephraim

**Specialization:** Computer Science - Fintech & Algorithmic Trading

## Problem Statement

Traditional KYC (Know Your Customer) processes create significant friction:
- **High abandonment rates:** 30-40% of users drop off during document upload
- **Security risks:** Storing identity documents creates data breach vulnerabilities
- **Redundant verification:** Users are verified repeatedly across different services
- **Market gap:** Banks have already performed rigorous identity verification but don't serve as identity providers

Current solutions like Open Banking focus on financial data sharing, not identity verification. Banks, despite being the most trusted verified entity, are not currently used as digital identity providers for external services.

## Solution

ProvIDe creates a digital identity layer that mediates between banks and digital services:

1. **Bank as Trust Anchor:** Uses existing bank KYC processes as the foundation
2. **Zero Document Upload:** Services receive verified identity assertions, not documents
3. **OAuth 2.0 / OpenID Connect:** Industry-standard protocols for secure authentication
4. **Privacy by Design:** System stores minimal metadata, not PII or financial data
5. **Regulatory Compliance:** GDPR-compliant, financial regulation compliant

### How It Works

1. User clicks "Authenticate with Bank" on a digital service
2. Secure redirect to bank via OpenFinance.ai
3. Strong authentication at bank (password, OTP, biometrics)
4. Bank confirms identity and returns digital assertion
5. Service receives verified identity confirmation—no documents needed

## Features

- ✅ **OAuth 2.0 / OpenID Connect** - Industry-standard authentication
- ✅ **PKCE (Proof Key for Code Exchange)** - Enhanced security for public clients
- ✅ **JWT/JWS Identity Assertions** - Cryptographically signed identity tokens
- ✅ **Scope-based Access Control** - Fine-grained permission management
- ✅ **Consent Management** - User-controlled data sharing
- ✅ **Audit Logging** - Comprehensive compliance tracking
- ✅ **Multi-bank Support** - Integration with multiple banks via OpenFinance.ai
- ✅ **RTL UI Support** - Right-to-left interface for Hebrew
- ✅ **Microservices Architecture** - Scalable, maintainable design
- ✅ **Token Management** - Secure token lifecycle (access, refresh, ID tokens)

## Architecture

### System Components

1. **Client Application** - Angular/React frontend with RTL support
2. **ProvIDe Platform** - Microservices-based backend
3. **Bank Authentication Layer** - OAuth 2.0 via OpenFinance.ai

### Microservices

- **OAuth Authorization Server** - OAuth 2.0/OpenID Connect flows, token issuance
- **Client Registry Service** - Service registration and onboarding
- **Consent Orchestration Service** - User consent management
- **OpenFinance.ai Integration Layer** - Bank OAuth flow orchestration
- **Identity Assertion Engine** - JWT assertion generation with cryptographic signatures
- **Audit & Compliance Module** - Event logging and compliance reporting

### Database Schema

- **users** - Authentication status, encrypted bank_user_id (no PII)
- **services** - Registered digital services (client_id, client_secret)
- **authorizations** - User consent records (scopes, timestamps)
- **tokens** - Token management (only hashes stored, not actual tokens)
- **audit_logs** - Immutable compliance logs
- **banks** - Bank configuration and integration settings

### Inter-Service Communication

- **RabbitMQ** - Asynchronous communication (audit logs, token revocation, notifications)
- **HTTP/REST** - Synchronous communication (OAuth flows, service registration)

## Technology Stack

### Backend
- **Runtime:** Node.js 18+
- **Framework:** Express.js
- **Database:** PostgreSQL 15+ with Prisma ORM
- **Caching:** Redis (token blacklist, rate limiting, session storage)
- **Message Queue:** RabbitMQ (async processing, event-driven communication)

### Frontend
- **Framework:** Angular or React
- **Styling:** Tailwind CSS
- **Features:** RTL support, OAuth redirect handling, responsive design

### Security Libraries
- **JWT:** jsonwebtoken / jose
- **OAuth:** node-oauth2-server / oauth2orize
- **Crypto:** Node crypto (built-in)
- **Hashing:** bcrypt

### Infrastructure
- **Containerization:** Docker
- **Load Balancing:** NGINX
- **Monitoring:** Prometheus, Grafana (TBD)
- **Logging:** Loki (with Grafana)

### Security & Encryption
- **TLS 1.3** - All external communications
- **AES-256-GCM** - Data encryption at rest
- **Bcrypt (cost 12)** - Client secret hashing
- **SHA-256** - Token hashing for revocation tracking
- **RSA-2048 / ECDSA-P256** - JWT signing keys
- **JWKS** - Secure key management with rotation

## Project Structure

```
provide/
├── index.html              # Main project documentation webpage
├── project-definition.txt  # Project definition template
├── הצעת פרויקט.txt        # Hebrew project proposal
├── מדריך-הצגה-עברית.md   # Hebrew presentation guide
├── README.md               # This file
└── favicons.html           # Favicon references
```

## Getting Started

### Prerequisites

- Node.js 18+ 
- PostgreSQL 15+
- Redis
- RabbitMQ
- Docker (optional, for containerized deployment)

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd provide
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your configuration
```

4. Set up the database:
```bash
npx prisma migrate dev
npx prisma generate
```

5. Start Redis and RabbitMQ:
```bash
# Redis
redis-server

# RabbitMQ (using Docker)
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

6. Run the application:
```bash
# Development
npm run dev

# Production
npm start
```

## Usage

### For Digital Services

1. **Register your service** via the Client Registry API
2. **Receive credentials** (client_id, client_secret)
3. **Integrate the "Authenticate with Bank" button** in your UI
4. **Implement OAuth 2.0 flow** to receive identity assertions
5. **Validate JWT tokens** using the JWKS endpoint

### For End Users

1. Click "Authenticate with Bank" on a digital service
2. Select your bank
3. Review consent screen (what data will be shared)
4. Authenticate with your bank (password, OTP, biometrics)
5. Approve consent
6. Return to the service with verified identity

## API Documentation

### OAuth 2.0 Endpoints

- `GET /oauth/authorize` - Authorization endpoint
- `POST /oauth/token` - Token exchange endpoint
- `GET /oauth/userinfo` - User info endpoint (OpenID Connect)
- `GET /.well-known/jwks.json` - JWKS endpoint for token validation

### Service Management

- `POST /api/services/register` - Register a new service
- `GET /api/services/:id` - Get service details
- `PUT /api/services/:id` - Update service configuration

### Consent Management

- `GET /api/consent/:authorization_id` - Get consent details
- `DELETE /api/consent/:authorization_id` - Revoke consent

For detailed API documentation, see the [project documentation](index.html).

## Security & Compliance

### Data Privacy

- **No PII Storage:** System stores only authentication metadata, not personal information
- **No Document Storage:** Identity documents are never stored
- **No Financial Data:** Bank financial data is never accessed or stored
- **Token Hashing:** Only token hashes are stored, not actual tokens

### Security Measures

- **OAuth 2.0 with PKCE** - Prevents authorization code interception
- **Short-lived Tokens** - Access tokens expire quickly, refresh tokens for renewal
- **Scope-based Access** - Fine-grained permission control
- **Rate Limiting** - Per-client, per-endpoint protection
- **CORS** - Strict origin validation
- **Audit Logging** - Immutable logs for compliance

### Compliance

- **GDPR Compliant** - Data minimization, consent management, right to deletion
- **Financial Regulation** - Adheres to financial service regulations
- **Audit Trails** - Complete event logging for regulatory requirements

## Use Cases

### 1. Lending & Financial Services
- Replace document upload and selfie verification
- Instant account opening
- Reduce abandonment by 30-40%

### 2. E-commerce & Trading Platforms
- Verify customer and seller identities
- Reduce identity fraud
- Open verified accounts quickly

### 3. Sensitive Services
- Insurance, crypto services
- Age/ citizenship-based services
- Highly regulated industries

## References

- [OAuth 2.0 Specification](https://oauth.net/2/)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [RFC 7636: PKCE](https://tools.ietf.org/html/rfc7636)
- [JSON Web Token (JWT)](https://jwt.io/)
- [OpenFinance.ai Documentation](https://openfinance.ai)
- [GDPR Compliance Guide](https://gdpr.eu/)

---

**Note:** This is an academic project. For production use, additional security audits, penetration testing, and regulatory approvals would be required.

**License:** [To be determined]

**Git Repository:** [Link to repository]
