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

After deployment, you'll need to configure the following environment variables in your Render dashboard:

1. `TD_MONGODB_URI`: This will be automatically set to your MongoDB connection string
2. `REDIS_URL`: This will be automatically set to your Redis connection string
3. `ELASTICSEARCH_URI`: If you want to use Elasticsearch, set this to your Elasticsearch instance URL
   - Note: You'll need to set up Elasticsearch separately as it's not provided by Render
   - You can use a service like Elastic Cloud or self-host your Elasticsearch instance

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
2. Verify all environment variables are set correctly
3. Ensure MongoDB and Redis services are running
4. Check the Trudesk logs for any specific error messages

## Support

For additional help:
- Render Documentation: https://render.com/docs
- Trudesk Documentation: https://docs.trudesk.io
- Trudesk GitHub: https://github.com/polonel/trudesk
