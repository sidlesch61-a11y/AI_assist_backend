# Render Deployment Guide

## Required Environment Variables

The following environment variables **must** be set in your Render dashboard for the backend to deploy successfully:

### Critical (Required)

1. **SECRET_KEY**
   - **Description**: Secret key for JWT token signing and encryption
   - **How to generate**: Run `python scripts/generate_secret_key.py` locally
   - **Example**: `your-generated-secret-key-here`
   - **Minimum length**: 32 characters
   - **⚠️ IMPORTANT**: Never use the default dev key in production!

2. **ENVIRONMENT**
   - **Description**: Application environment
   - **Values**: `production`, `staging`, or `development`
   - **Default**: `development` (not recommended for production)

### Database Configuration

3. **POSTGRES_HOST**
   - **Description**: PostgreSQL database host
   - **For Render**: Usually provided by Render's PostgreSQL service
   - **Example**: `dpg-xxxxx-a.oregon-postgres.render.com`

4. **POSTGRES_PORT**
   - **Description**: PostgreSQL port
   - **Default**: `5432`

5. **POSTGRES_USER**
   - **Description**: PostgreSQL username
   - **For Render**: Usually provided by Render's PostgreSQL service

6. **POSTGRES_PASSWORD**
   - **Description**: PostgreSQL password
   - **For Render**: Usually provided by Render's PostgreSQL service
   - **⚠️ IMPORTANT**: Keep this secret!

7. **POSTGRES_DB**
   - **Description**: PostgreSQL database name
   - **For Render**: Usually provided by Render's PostgreSQL service
   - **Example**: `vehicle_ai_db`

### Optional (Recommended)

8. **REDIS_URL**
   - **Description**: Redis connection URL for caching and sessions
   - **Default**: `redis://redis:6379/0`
   - **For Render**: Use Render's Redis service URL if available

9. **OPENAI_API_KEY**
   - **Description**: OpenAI API key for AI features
   - **Required**: Only if using AI features
   - **Example**: `sk-...`

10. **DEBUG**
    - **Description**: Enable debug mode
    - **Values**: `true` or `false`
    - **Default**: `true` (set to `false` in production!)

## Setting Environment Variables in Render

1. Go to your Render dashboard
2. Select your backend service
3. Navigate to **Environment** tab
4. Click **Add Environment Variable**
5. Add each variable with its value
6. Click **Save Changes**
7. Redeploy your service

## Quick Setup Script

You can generate a secure SECRET_KEY by running:

```bash
cd backend
python scripts/generate_secret_key.py
```

Copy the generated key and add it to Render as `SECRET_KEY`.

## Common Issues

### Error: "SECRET_KEY Field required"

**Solution**: Add `SECRET_KEY` environment variable in Render dashboard.

### Error: "SECRET_KEY must be at least 32 characters"

**Solution**: Generate a new SECRET_KEY using the script above.

### Error: "SECRET_KEY must be set to a secure value in production"

**Solution**: 
1. Set `ENVIRONMENT=production` in Render
2. Generate and set a secure `SECRET_KEY` (not starting with "dev-secret-key")

## Database Setup

If using Render's PostgreSQL service:

1. Create a PostgreSQL database in Render
2. Copy the connection details
3. Set the environment variables:
   - `POSTGRES_HOST`
   - `POSTGRES_USER`
   - `POSTGRES_PASSWORD`
   - `POSTGRES_DB`
   - `POSTGRES_PORT` (usually 5432)

## Migration

After setting up the database, run migrations:

```bash
cd backend
alembic upgrade head
```

Or configure Render to run migrations automatically on deploy.

## Verification

After deployment, check the logs to ensure:
- ✅ No "SECRET_KEY" errors
- ✅ Database connection successful
- ✅ Application started successfully

## Security Checklist

- [ ] `SECRET_KEY` is set and secure (32+ characters)
- [ ] `ENVIRONMENT=production` is set
- [ ] `DEBUG=false` is set
- [ ] Database credentials are secure
- [ ] All sensitive keys are in environment variables (not in code)

