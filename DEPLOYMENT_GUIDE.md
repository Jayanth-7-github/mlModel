# Render Deployment Guide for Music Genre Classification Django App

## Step 1: Prepare Your Local Repository

Make sure all changes are committed to git:

```bash
git init
git add .
git commit -m "Initial commit"
```

## Step 2: Push to GitHub

1. Create a new repository on GitHub
2. Push your code:

```bash
git remote add origin https://github.com/yourusername/Music-Genre-Classification-Django.git
git branch -M main
git push -u origin main
```

## Step 3: Create Render Account

1. Go to https://render.com
2. Sign up with GitHub
3. Authorize Render to access your repositories

## Step 4: Deploy on Render

### Option A: Using render.yaml (Recommended)

1. Log in to Render Dashboard
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Select your repository and branch
5. Render will automatically detect render.yaml
6. Set environment variables:
   - `DEBUG`: False
   - `SECRET_KEY`: Generate a strong key (can be auto-generated)
   - `ALLOWED_HOSTS`: yourapp.onrender.com

### Option B: Manual Configuration via Web Interface

1. Click "New +" → "Web Service"
2. Connect your GitHub repository
3. Fill in the following:
   - **Name**: music-genre-classifier
   - **Environment**: Python 3
   - **Build Command**:
     ```
     pip install -r requirements.txt && python manage.py migrate && python manage.py collectstatic --noinput
     ```
   - **Start Command**:
     ```
     gunicorn MusicClassification.wsgi
     ```
   - **Instance Type**: Select based on needs

4. Add Environment Variables:
   - `DEBUG`: False
   - `SECRET_KEY`: (Generate a secure key)
   - `ALLOWED_HOSTS`: yourapp.onrender.com

## Step 5: Configure Environment Variables

In Render Dashboard:

1. Go to your service
2. Settings → Environment
3. Add these variables:

```
DEBUG=False
SECRET_KEY=your-very-long-random-secret-key-here
ALLOWED_HOSTS=yourapp.onrender.com,localhost
```

To generate a SECRET_KEY, run:

```python
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```

## Step 6: Monitor Deployment

1. Go to your Render service
2. Watch the "Logs" tab for deployment progress
3. Once "Deploy successful" appears, your app is live!

## Step 7: Access Your App

Your app will be available at: `https://yourapp.onrender.com`

## Troubleshooting

### Application fails to start

- Check the logs in Render dashboard
- Ensure all dependencies in requirements.txt are correct
- Verify environment variables are set

### Static files not loading

- Make sure STATIC_ROOT is set (it is in our config)
- Clear browser cache
- Check if whitenoise is properly configured

### Database issues

- For Render free tier, SQLite works fine
- Migrations run automatically during build
- You can't modify the database after deployment on free tier

### Port issues

- Render automatically assigns port 10000 (or similar)
- The app should work without changing anything

## Important Notes

⚠️ **Free Tier Limitations:**

- App spins down after 15 minutes of inactivity
- Limited compute resources
- SQLite database (no persistence across redeploys)

✅ **For Production Use:**

- Upgrade to paid tier for better performance
- Consider using PostgreSQL for database
- Enable auto-deploy for continuous deployment
- Set up monitoring and alerts

## File Reference

Created/Modified files for deployment:

- `requirements.txt` - Updated with correct versions
- `Procfile` - Specifies how to run the app
- `render.yaml` - Render configuration file
- `build.sh` - Build script for Render
- `.env.example` - Example environment variables
- `MusicClassification/settings.py` - Updated for production

## Next Steps

After successful deployment:

1. Test file upload functionality
2. Monitor logs for any errors
3. Consider setting up a PostgreSQL database for production
4. Enable auto-deploy on push for continuous deployment
