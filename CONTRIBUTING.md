# Contributing Guide

Terima kasih telah tertarik untuk berkontribusi pada **Digital Invitation Platform**.

Dokumen ini menjelaskan standar pengembangan, alur kerja Git, dan aturan kontribusi yang wajib diikuti agar kualitas kode tetap konsisten.

---

# Project Overview

Digital Invitation Platform adalah platform SaaS dan E-Commerce Undangan Digital yang dibangun menggunakan arsitektur modern berbasis:

- Laravel 11
- PostgreSQL
- Redis
- React / Next.js
- TailwindCSS
- Docker
- REST API
- Clean Architecture
- Service Repository Pattern

---

# Branch Strategy

Gunakan Git Flow sederhana.

```
main
│
├── develop
│
├── feature/authentication
├── feature/payment
├── feature/builder
├── feature/storefront
├── feature/dashboard
│
├── fix/login
├── hotfix/payment
```

## Branch

### main

Berisi kode Production.

Tidak boleh commit langsung.

---

### develop

Berisi hasil integrasi seluruh feature.

---

### feature/*

Digunakan untuk mengembangkan fitur baru.

Contoh

```
feature/auth
feature/payment
feature/template
feature/rsvp
```

---

### fix/*

Perbaikan bug.

---

### hotfix/*

Perbaikan bug production yang bersifat kritis.

---

# Commit Message Convention

Gunakan Conventional Commits.

Contoh

```
feat(auth): add google login

feat(builder): autosave invitation

fix(payment): resolve duplicate webhook

refactor(order): simplify service layer

docs(api): update authentication flow

test(builder): add invitation tests

style: apply laravel pint

chore: update dependencies
```

---

# Pull Request Rules

Setiap Pull Request harus:

- memiliki deskripsi jelas
- menjelaskan perubahan
- lolos testing
- lolos linting
- tidak memiliki conflict
- direview minimal 1 orang

Template PR

```
## Description

Ringkasan perubahan

## Type

- Feature
- Fix
- Refactor
- Documentation

## Checklist

- [ ] Build Success
- [ ] Test Passed
- [ ] No Conflict
- [ ] Documentation Updated
```

---

# Coding Standards

Backend

- PSR-12
- Laravel Pint
- PHPStan Level 5+
- Service Repository Pattern
- Slim Controller
- Fat Service
- API Resource
- Form Request Validation

Frontend

- ESLint
- Prettier
- TypeScript Strict Mode
- Atomic Components
- Reusable Hooks

---

# Architecture Rules

Dilarang menulis Business Logic di Controller.

Semua Business Logic wajib berada di:

```
app/Services
```

Akses database dilakukan melalui:

```
Repositories
```

Controller hanya:

- menerima request
- memanggil service
- mengembalikan response

---

# Database Rules

- Tidak boleh menggunakan SELECT *
- Gunakan eager loading
- Gunakan transaction untuk operasi multi tabel
- Gunakan UUID
- Gunakan migration
- Jangan mengubah data payment yang sudah PAID

---

# API Rules

Semua endpoint wajib:

- menggunakan Form Request
- menggunakan API Resource
- mengembalikan JSON
- menggunakan HTTP Status yang benar
- menggunakan JWT Authentication

---

# Testing

Setiap fitur baru harus memiliki testing.

Jenis testing:

- Unit Test
- Feature Test
- Integration Test

Minimal command

```
php artisan test
```

---

# Code Review Checklist

Reviewer akan memeriksa:

- Architecture
- Naming
- Readability
- Performance
- Security
- Validation
- Authorization
- Test Coverage
- Documentation

---

# Security

Jangan pernah melakukan commit:

- .env
- API Key
- JWT Secret
- Midtrans Server Key
- Google OAuth Secret
- AWS Secret

Gunakan Environment Variables.

---

# Issue Reporting

Saat membuat Issue sertakan:

- Deskripsi
- Langkah reproduksi
- Screenshot
- Log Error
- Environment

---

# Documentation

Jika menambahkan fitur baru, dokumentasi berikut wajib diperbarui jika relevan:

- PRD
- SRS
- SAD
- API Specification
- Database Architecture
- CHANGELOG

---

# Development Workflow

```
Issue
    ↓

Create Feature Branch
    ↓

Develop
    ↓

Testing
    ↓

Lint
    ↓

Commit
    ↓

Push
    ↓

Pull Request
    ↓

Review
    ↓

Merge to develop
    ↓

Release
    ↓

Merge to main
```

---

# License

Dengan berkontribusi pada proyek ini, Anda menyetujui bahwa seluruh kontribusi akan mengikuti lisensi proyek yang berlaku.

---

Terima kasih telah membantu mengembangkan Digital Invitation Platform.
