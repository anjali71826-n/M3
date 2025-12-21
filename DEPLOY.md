# Deployment Guide

This project is ready to be deployed to Vercel. It contains two main interfaces:
1. **Admin Dashboard**: Hosted at `/` (`index.html`)
2. **Public Chat Interface**: Hosted at `/chat` (`chat.html`)

## Option 1: Deploy using Vercel Dashboard (Recommended)

1. **Push to GitHub**: ensure your latest code is pushed to your GitHub repository.
2. **Go to Vercel**: Log in to [vercel.com](https://vercel.com).
3. **Import Project**: Click "Add New..." -> "Project".
4. **Select Repository**: specificy the `M3` repository.
5. **Configure**: Vercel will automatically detect it as a static site. No override is needed.
6. **Deploy**: Click Deploy.

## Option 2: Deploy using Vercel CLI

If you have the Vercel CLI installed:

1. Open your terminal in this directory.
2. Run:
   ```bash
   npx vercel
   ```
3. Follow the prompts (Select Scope, Link to existing project: No, etc.).
4. Use default settings for everything.

## Routes
Once deployed, your app will be available at:
- `https://your-project.vercel.app/` -> The Admin Dashboard
- `https://your-project.vercel.app/chat` -> The Chat Interface
