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

1. `TD_MONGODB_URL`: MongoDB connection URL (automatically constructed using database service credentials)
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

## Database Connection

The MongoDB connection URL is constructed using the following format:
```
mongodb://${username}:${password}@${host}:${port}/${database}
```

All these values are automatically populated from the MongoDB service credentials.

## Port Configuration

The application is configured to explicitly bind to the PORT environment variable using:
```
PORT=$PORT yarn start
```

This ensures Render can properly detect and route traffic to your application.

## Node.js Version

The application uses Node.js 16.20.0 LTS for compatibility with node-sass and other dependencies. This version is set via the NODE_VERSION environment variable.

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

1. Database Connection Issues:
   - Check the MongoDB service is running in your Render dashboard
   - Verify the MongoDB connection URL is correctly constructed
   - Look for any authentication errors in the logs

2. Port Binding Issues:
   - Check the logs for any port-related errors
   - Verify the PORT environment variable is being properly used
   - Ensure no other service is using the same port

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
