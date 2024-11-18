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
   - MongoDB database
   - Redis cache

## Environment Variables

The following environment variables will be automatically configured:

1. `TD_MONGODB_URL`: MongoDB connection URL (automatically linked)
2. `REDIS_URL`: Redis connection URL (automatically linked)
3. `TRUDESK_JWTSECRET`: Automatically generated secure token
4. `NODE_ENV`: Set to "production"
5. `PORT`: Set to 8118
6. `TRUDESK_DOCKER`: Set to "true"
7. `NODE_VERSION`: Set to 16.20.0 for compatibility with node-sass

Optional variables you may need to configure manually:
1. `ELASTICSEARCH_URI`: If you want to use Elasticsearch, set this to your Elasticsearch instance URL
   - Note: You'll need to set up Elasticsearch separately as it's not provided by Render
   - You can use a service like Elastic Cloud or self-host your Elasticsearch instance

## Node.js Version

The application uses Node.js 16.20.0 LTS for compatibility with node-sass and other dependencies. This version is set via the NODE_VERSION environment variable in the `render.yaml` configuration.

## Post-Deployment Setup

1. Once deployed, visit your Trudesk URL (shown in the Render dashboard)
2. You'll be redirected to the installation page on first run
3. Follow the setup wizard to:
   - Create your admin account
   - Configure your email settings
   - Set up your help desk preferences

## Monitoring

- Monitor your application logs in the Render dashboard
- Check the health check endpoint at `/` to ensure the service is running
- Use Render's metrics to monitor resource usage

## Troubleshooting

If you encounter any issues:

1. Check the application logs in the Render dashboard
2. Verify all environment variables are properly linked:
   - MongoDB URL should be linked from the trudesk-mongodb service
   - Redis URL should be linked from the trudesk-redis service
3. Ensure MongoDB and Redis services are running
4. Check the Trudesk logs for any specific error messages
5. Verify Node.js version is correctly set to 16.20.0

## Support

For additional help:
- Render Documentation: https://render.com/docs
- Trudesk Documentation: https://docs.trudesk.io
- Trudesk GitHub: https://github.com/polonel/trudesk
