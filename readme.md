# Next.js Deployment on cPanel - Mahfuz

This guide explains how to deploy a **Next.js application on cPanel using Node.js Application Manager and Next.js Standalone Output**.

---

## Prerequisites

Before starting, make sure:

* cPanel hosting supports **Node.js**
* **Setup Node.js App** is available in cPanel
* A supported Node.js version is installed
* You have access to the project's files
* The Next.js project builds successfully with `npm run build`

---

# Deployment Process

## Step 1 — Create the Node.js Application

First, create a Node.js application from:

**cPanel → Setup Node.js App → Create Application**

Configure the application according to your hosting environment.

Example:

```text
Node.js Version: 20.x
Application Mode: Production
Application Root: your-project
Application URL: your-domain.com
```

> The exact available Node.js versions and fields may vary depending on your hosting provider.

---

## Step 2 — Configure Next.js Standalone Output

Open the project's `next.config.js` file and add:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'standalone',
};

module.exports = nextConfig;
```

If your project already has other Next.js configurations, keep them and only add:

```js
output: 'standalone'
```

Example:

```js
const nextConfig = {
  output: 'standalone',

  // Other existing configurations
};

module.exports = nextConfig;
```

---

## Step 3 — Build the Project

Run the following command in the project directory:

```bash
npm run build
```

After a successful build, Next.js will generate the `.next` directory.

The important directory is:

```text
.next/standalone
```

---

## Step 4 — Upload the Standalone Build

Inside the generated `.next` directory, you will find:

```text
.next/
└── standalone/
```

Upload the **contents of the `standalone` folder** to the root directory of your Node.js application in cPanel.

The standalone build contains the production server and the required application files.

---

## Step 5 — Upload the `public` Folder

Upload the project's `public` folder to the application root.

Example:

```text
your-project/
├── public/
└── server.js
```

The `public` folder is required for static assets such as:

* Images
* Icons
* Fonts
* Other public files

---

## Step 6 — Upload `.next/static`

From the local project, locate:

```text
.next/static
```

Upload the entire `static` folder into the `.next` directory on cPanel.

The final structure should contain:

```text
.next/
└── static/
```

This step is important because Next.js uses these static files for:

* JavaScript bundles
* CSS
* Optimized assets
* Build-generated frontend resources

---

## Step 7 — Make Sure `server.js` Exists

The standalone build generates:

```text
.next/standalone/server.js
```

Make sure this `server.js` file exists in the **root directory of the Node.js application**.

Example:

```text
your-project/
├── server.js
├── public/
├── .next/
└── ...
```

The `server.js` file is the production server entry point for the standalone Next.js application.

---

## Step 8 — Install Dependencies

If your cPanel Node.js application requires dependencies to be installed, open the application's terminal/SSH environment and run:

```bash
npm install --omit=dev
```

> Depending on how the standalone build and your hosting environment are configured, you may not need to upload your local `node_modules` folder.

---

## Step 9 — Configure Environment Variables

If your project uses environment variables, add them from:

**cPanel → Setup Node.js App → Environment Variables**

For example:

```text
NODE_ENV=production
DATABASE_URL=your_database_url
NEXT_PUBLIC_API_URL=https://api.example.com
```

Do not upload sensitive `.env` files publicly.

---

## Step 10 — Restart the Node.js Application

After uploading all required files and configuring the environment variables:

Go to:

**cPanel → Setup Node.js App**

Find your application and click:

**Restart**

---

# Final Directory Structure

The application should approximately look like this:

```text
your-project/
│
├── .next/
│   └── static/
│
├── public/
│
├── server.js
│
├── node_modules/
│
└── package.json
```

The exact structure may vary slightly depending on the project and cPanel Node.js configuration.

---

# Deployment Checklist

Before opening the website, verify:

* [ ] Node.js application has been created
* [ ] `output: 'standalone'` has been added to `next.config.js`
* [ ] `npm run build` completed successfully
* [ ] `.next/standalone` contents uploaded
* [ ] `public` folder uploaded
* [ ] `.next/static` uploaded
* [ ] `server.js` exists in the application root
* [ ] Required environment variables are configured
* [ ] Dependencies are installed if required
* [ ] Node.js application has been restarted
* [ ] Domain is correctly connected to the Node.js application

---

# Important Notes

### 1. Do not upload the entire local `.next` folder blindly

For standalone deployment, make sure the required standalone files are copied correctly and that:

```text
.next/static
```

is also available in the final deployment.

### 2. Do not expose environment variables

Never commit sensitive `.env` files or database credentials to GitHub.

### 3. Restart after deployment

Whenever you upload a new build, restart the Node.js application from cPanel.

### 4. Build locally before uploading

