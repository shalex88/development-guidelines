# Deployment Strategy

This document outlines how we package, aggregate, and deploy a multi-service edge system. The goals are: repeatable builds, consistent versioning across services, controlled deployments, and reduced risk through ordered rollout.

## Implement Meta-Repository Architecture
```bash
<my-multi-service>-system/
├── submodules/
├───── <main-target>/
├──────── frontend/
├──────── service_a/
├───── <secondary-target>/
├──────── service_b/
├── build/
├──── scripts/
├────── build_all.sh
├────── create_meta_package.sh
├──── config/
├── package/
├──── meta-package/
├──── output/
├── deploy/
├──── deploy.sh
├── tests/
├──── integration/
├──── system/
├── docs/
├── ci-pipeline.yml
├── .gitmodules
├── VERSION
├── CHANGELOG.md
└── README.md
```

## Package with Native Package Manager
Package each service using native package managers (e.g., DEB for Debian/Ubuntu) to enable atomic versioning, dependency management, and simplified rollbacks.

## Aggregate Service Packages into Meta-Package
Create a meta-package that declares dependencies on all service packages, ensuring the entire system is deployed as a cohesive unit with consistent versions.

## Deploy with Push-Based Strategy
Utilize a push-based deployment strategy where the deployment server pushes updates to the main target.

## Use Bottom-Up Deployment Approach
Update secondary targets first before updating the main target. Update dependent services before updating the main application.