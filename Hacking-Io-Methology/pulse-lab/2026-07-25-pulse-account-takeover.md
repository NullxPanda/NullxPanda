# Pulse Lab - Account Takeover via Password Reset Token Leak
Date: 2026-07-25
Target: https://2d5r07b0mh88.ctfhub.io/pulse
Difficulty: Easy
Objective: Take over Pulse employee account -> reach staff console with full patient charts

## Root Cause
POST /api/auth/forgot returns the live reset token in the response body under notification.preview.token

## Impact
Full employee account takeover -> unrestricted access to every patient chart

## Steps to Reproduce
1. Enumerate staff emails via GET /api/posts (Newsroom authors)
2. POST /api/auth/forgot with any staff email
3. Extract token from notification.preview.token
4. POST /api/auth/reset with token + new password
5. POST /api/auth/login -> receive p_sid session
6. Access /portal and GET /api/staff/patients

## Key Endpoints
- GET  /api/posts
- POST /api/auth/forgot
- POST /api/auth/reset
- POST /api/auth/login
- GET  /api/me
- GET  /api/staff/patients
- GET  /portal  (Staff Console)

## Methodology Applied
1. Scope & Objective Lock
2. Exhaustive Passive Recon (headers + full source)
3. Follow public surfaces (Newsroom -> API)
4. Highest-value auth flow first (password reset)
5. Convert leak -> session
6. Locate protected resource with authenticated session

## Lessons
- Password reset endpoints that return tokens in the response are critical
- Public content (blog authors) is high-value recon
- Always test the forgot-password flow before anything else on employee portals
