# ChatGPT Application Setup and Deployment Guide

This guide outlines the steps to create and deploy a ChatGPT application using Assistant UI, Google's Gemini API, and Vercel.

## Prerequisites

Before you begin, ensure you have the following installed:

*   **VS Code:** [VSCODE Link](https://code.visualstudio.com/)
*   **Node.js:** [Node JS Link](https://nodejs.org/) (Recommended version: Latest LTS)
*   **Vercel CLI:** (Install globally using `npm install -g vercel`)
*   **Git:** (For version control and deployment to Vercel)

## Setup Instructions

1.  **Create Project Folder:**

    *   Create a new folder named `ChatGPT`.
    *   Open the `ChatGPT` folder in VS Code.

2.  **Initialize Assistant UI:**

    *   Open the VS Code terminal (View -> Terminal).
    *   Run the following command to initialize the Assistant UI project:

        ```bash
        npx assistant-ui@latest create
        ```

    *   Follow the prompts to configure your project.  Choose the default options if you're unsure.  This will create a subfolder named `my-app` within the `ChatGPT` folder.

3.  **Navigate to the Project Directory:**

    *   In the VS Code terminal, navigate to the `my-app` directory:

        ```bash
        cd my-app
        ```

4.  **Install Dependencies:**

    *   Install the necessary provider SDKs:

        ```bash
        npm install ai @assistant-ui/react-ai-sdk @ai-sdk/google
        ```

5.  **Configure the Chat Route:**

    *   Navigate to the `/app/api/chat/route.ts` file within your project.
    *   Replace the existing content of the file with the following code:

        ```typescript
        import { google } from "@ai-sdk/google";
        import { streamText } from "ai";

        export const maxDuration = 30;

        export async function POST(req: Request) {
          const { messages } = await req.json();
          const result = streamText({
            model: google("gemini-2.0-flash"),
            messages,
          });
          return result.toDataStreamResponse();
        }
        ```

6.  **Obtain Google AI API Key:**

    *   Go to [Google AI Studio](APIKEY - Replace with the actual link to Google AI Studio).
    *   Create a new API key.  Make sure to restrict the key if possible for security.

7.  **Create `.env.local` File:**

    *   In the root of your `my-app` directory (where `package.json` is located), create a file named `.env.local`.

8.  **Add API Key to `.env.local`:**

    *   Add the following line to your `.env.local` file, replacing `"Your_API_KEY"` with the actual API key you obtained from Google AI Studio:

        ```
        GOOGLE_GENERATIVE_AI_API_KEY="Your_API_KEY"
        ```

    **Important:**  Never commit your `.env.local` file to your Git repository.  It contains sensitive information.  Make sure `.env.local` is in your `.gitignore` file.

9.  **Run the Development Server:**

    *   In the VS Code terminal, run the following command to start the development server:

        ```bash
        npm run dev
        ```

    *   The application will typically be accessible at `http://localhost:3000`.  Open this address in your web browser to test the application.

## Deployment to Vercel

1.  **Build the Application:**

    *   In the VS Code terminal, run the following command to build the application for production:

        ```bash
        npm run build
        ```

2.  **Initialize Git Repository (if not already done):**

    *   If you haven't already, initialize a Git repository in your `ChatGpt` folder:

        ```bash
        git init
        git add .
        git commit -m "Initial commit"
        ```

3.  **Create a GitHub Repository:**

    *   Create a new repository on GitHub.

4.  **Push to GitHub:**

    *   Connect your local repository to your GitHub repository and push the code:

        ```bash
        git remote add origin <your_github_repository_url>
        git branch -M main
        git push -u origin main
        ```

5.  **Deploy to Vercel:**

    *   **Important:** Make sure you are deploying the `my-app` folder, not the `ChatGpt` folder.
    *   You have two options for deploying to Vercel:

        *   **Vercel CLI:**  Run `vercel` in the `my-app` directory.  Follow the prompts to link your project to your Vercel account and your GitHub repository.
        *   **Vercel Dashboard:**  Log in to your Vercel account at [https://vercel.com/](https://vercel.com/).  Click "Add New..." and import your project from your GitHub repository.

    *   When prompted, select the **Next.js** option as the framework.

6.  **Configure Environment Variables in Vercel:**

    *   In your Vercel project dashboard, go to the "Settings" tab and then "Environment Variables".
    *   Add the `GOOGLE_GENERATIVE_AI_API_KEY` environment variable and set its value to your Google AI API key.  This is crucial for the application to function correctly in production.

7.  **Wait for Deployment:**

    *   Vercel will automatically build and deploy your application.  This process may take a few minutes.

8.  **Access Your Deployed Application:**

    *   Once the deployment is complete, Vercel will provide you with a unique URL where your application is live.  You can find this URL in your Vercel project dashboard.

## Troubleshooting

*   **API Key Issues:**  Double-check that your `GOOGLE_GENERATIVE_AI_API_KEY` is correctly set in both your `.env.local` file (for local development) and in the Vercel environment variables (for production).
*   **Dependency Errors:**  Ensure that all dependencies are correctly installed by running `npm install` in the `my-app` directory.
*   **Deployment Errors:**  Review the Vercel deployment logs for any error messages.  These logs can provide valuable clues about the cause of the problem.
*   **Assistant UI Issues:** Refer to the [AssistantUI](AssistantUI - Replace with the actual link to AssistantUI documentation) documentation for troubleshooting.

## Important Considerations

*   **Security:**  Protect your Google AI API key.  Do not expose it in client-side code or commit it to your Git repository.  Use environment variables to store sensitive information.
*   **Rate Limiting:**  Be aware of the rate limits imposed by the Google AI API.  Implement appropriate error handling and retry mechanisms in your application.
*   **Cost:**  Monitor your usage of the Google AI API to avoid unexpected costs.
*   **Vercel Hobby Plan:** The Vercel Hobby plan has limitations. If you need more resources or features, consider upgrading to a paid plan.

This guide provides a comprehensive overview of the setup and deployment process.  If you encounter any issues, consult the documentation for Assistant UI, Google AI Studio, and Vercel for further assistance. Good luck!
