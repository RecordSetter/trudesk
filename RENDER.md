# Deploying Trudesk to Render

This guide explains how to deploy Trudesk to Render using the Blueprint specification.

## Prerequisites

1. A Render account
2. Your Trudesk repository connected to Render

## Deployment Steps

1. Push the `render.yaml` file to your repository
2. In your Render dashboard, click "New +" and select "Blueprint"
3. Connect your repository if you haven't already
4. Render will automatically detect the `render.yaml` file and create the following services:
   - Web service running the Trudesk application
   - MongoDB private service (running in Docker)
   - Redis cache

## Services Configuration

### Main Application
- Node.js web service running the Trudesk application
- Uses Node.js 16.20.0 for compatibility with node-sass
- Binds to port 10000 as required by Render
- Free tier plan

### MongoDB
- Runs as a private service using mongo:4.4 Docker image
- Docker runtime for containerization
- Includes a 10GB persistent disk for data storage
- Accessible internally via mongodb://trudesk-mongodb:27017/trudesk
- Free tier plan

### Redis
- Managed Redis service
- Used for caching and session storage
- Automatically configured and linked
- Free tier plan

## Environment Variables

The following environment variables will be automatically configured:

1. `TD_MONGODB_URL`: Set to connect to the MongoDB private service
2. `REDIS_URL`: Redis connection URL (automatically linked)
3. `TRUDESK_JWTSECRET`: Automatically generated secure token
4. `NODE_ENV`: Set to "production"
5. `PORT`: Set to 10000 (Render's default port)
6. `TRUDESK_DOCKER`: Set to "true"
7. `NODE_VERSION`: Set to 16.20.0 for compatibility with node-sass

Optional variables you may need to configure manually:
1. `ELASTICSEARCH_URI`: If you want to use Elasticsearch, set this to your Elasticsearch instance URL
   - Note: You'll need to set up Elasticsearch separately as it's not provided by Render
   - You can use a service like Elastic Cloud or self-host your Elasticsearch instance

## Port Configuration

The application is explicitly configured to:
1. Use port 10000 (Render's default port) in both:
   - Environment variable: `PORT=10000`
   - Start command: `PORT=10000 yarn start`
2. Bind to host '0.0.0.0' for proper Render compatibility
3. Use the /healthz endpoint for health checks

## Data Persistence

MongoDB data is stored on a persistent disk:
- Size: 10GB
- Mount path: /data/db
- Ensures data survives container restarts

## Post-Deployment Setup

1. Once deployed, visit your Trudesk URL (shown in the Render dashboard)
2. You'll be redirected to the installation page on first run
3. Follow the setup wizard to:
   - Create your admin account
   - Configure your email settings
   - Set up your help desk preferences

## Monitoring

- Monitor your application logs in the Render dashboard
- Check the health check endpoint at `/healthz` to ensure the service is running
- Use Render's metrics to monitor resource usage

## Troubleshooting

1. MongoDB Issues:
   - Check the MongoDB private service logs
   - Verify the persistent disk is properly mounted
   - Ensure the MongoDB connection URL is correct

2. Port Binding Issues:
   - Verify the application is binding to port 10000
   - Check logs for any port-related errors
   - Ensure both the environment variable and start command specify PORT=10000

3. Node.js Issues:
   - Verify NODE_VERSION is set to 16.20.0
   - Check for any node-sass compilation errors
   - Look for any dependency-related errors in the logs

4. General Issues:
   - Review the application logs in the Render dashboard
   - Check the service status in your Render dashboard
   - Verify all environment variables are set correctly

## Support

For additional help:
- Render Documentation: https://render.com/docs
- Trudesk Documentation: https://docs.trudesk.io
- Trudesk GitHub: https://github.com/polonel/trudesk
