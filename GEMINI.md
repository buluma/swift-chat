# SwiftChat Project Overview

## Project Summary

SwiftChat is a fast and responsive cross-platform AI assistant developed with **React Native** for the frontend, targeting Android, iOS, and macOS platforms. The backend is a **Python** application utilizing **FastAPI**, deployed as an **AWS Lambda** function via **API Gateway**. It supports multiple AI model providers including Amazon Bedrock, Ollama, DeepSeek, OpenAI, and other OpenAI-compatible models.

Key features include:
- Real-time streaming conversations
- AI image generation
- Instant web app creation, editing, and sharing
- Web search for real-time information retrieval
- Comprehensive multimodal analysis (text, image, document, video)
- Robust privacy and security features

## Architecture

The project follows a client-server architecture.

-   **Client (Frontend):** A React Native application handles the user interface and interactions. It's built with TypeScript/JavaScript and includes native modules for platform-specific functionalities.
-   **Server (Backend):** A Python application built with FastAPI serves as the API backend. It's containerized and deployed as an AWS Lambda function, exposed via Amazon API Gateway. This server handles requests from the client, interacts with various AI model providers (e.g., Amazon Bedrock), and manages API key authentication.

## Technologies Used

### Frontend
-   **Framework:** React Native
-   **Languages:** TypeScript, JavaScript, Objective-C++ (iOS native modules), Java (Android native modules)
-   **Navigation:** `@react-navigation/native`
-   **State Management/Storage:** `react-native-mmkv` for fast local storage
-   **Utilities:** `react-native-compressor` for image handling, `patch-package` for dependency patches

### Backend
-   **Language:** Python
-   **Web Framework:** FastAPI
-   **AWS SDK:** `boto3`
-   **ASGI Server:** `uvicorn`
-   **Deployment:** AWS Lambda, Amazon API Gateway, AWS ECR (Elastic Container Registry), AWS CloudFormation

## Building and Running

### Frontend (React Native Application)

Navigate to the `react-native/` directory for all frontend operations.

1.  **Install Dependencies:**
    ```bash
    cd react-native
    npm install
    ```

2.  **Start React Native Development Server:**
    ```bash
    npm start
    ```

3.  **Run on Android:**
    *   Ensure an Android emulator is running or a device is connected.
    ```bash
    npm run android
    ```

4.  **Run on iOS:**
    *   **First-time setup (install native dependencies):**
        ```bash
        cd ios && pod install && cd ..
        ```
    *   Ensure an iOS simulator is running or a device is connected.
    ```bash
    npm run ios
    ```

5.  **Run on macOS (Mac Catalyst):**
    *   Ensure the React Native development server is running (`npm start`).
    *   Open `ios/SwiftChat.xcworkspace` in Xcode.
    *   Change the build destination to `My Mac (Mac Catalyst)`.
    *   Click the ▶ Run button in Xcode.

### Backend (Python/AWS Lambda)

Navigate to the `server/` directory for backend operations.

1.  **Build and Push Docker Image to ECR:**
    ```bash
    cd server/scripts
    bash ./push-to-ecr.sh
    ```
    Follow the prompts to configure ECR repository name, image tag, and AWS region. Copy the resulting ECR image URI.

2.  **Deploy AWS CloudFormation Stack:**
    *   The CloudFormation template is located at `server/template/SwiftChatLambda.template`.
    *   Use this template in the AWS CloudFormation console to create a new stack.
    *   Provide the ECR image URI obtained in the previous step as the `ContainerImageUri` parameter.
    *   The deployment will set up API Gateway and Lambda functions for the backend.

## Development Conventions

-   **Code Formatting:** Prettier (`prettier`) is used for code formatting, and ESLint (`eslint`) for linting in the React Native project.
-   **Native Module Patching:** `patch-package` is used to apply patches to `node_modules` dependencies, visible in `react-native/patches/`.
-   **iOS-specific:**
    -   Uses CocoaPods (`Podfile`) for managing native iOS dependencies.
    -   Includes custom patches (`RCTNetworkingPatch.h/.m`, `RCTTextInputPatch.h/.mm`) for React Native networking and text input behavior.
    -   Swift versioning is managed in `Podfile`, with Swift 6.0 generally, and Swift 5.0 for specific pods like `react-native-compressor` to avoid compatibility issues.
-   **Android-specific:**
    -   Uses Gradle for the build system.
    -   `proguard-rules.pro` is used for code minification in release builds (`enableProguardInReleaseBuilds = true`).
    -   The Hermes JavaScript engine is utilized for improved performance.
    -   Custom logic in `react-native/android/app/build.gradle` handles duplicate classes from `react-android` JAR files during release builds to prevent conflicts.
-   **Error Handling:** The `react-native/src/utils/ErrorUtils.ts` file suggests a custom error handling configuration for the frontend.
-   **Imports:** ES6 `import` syntax is used in the React Native codebase.
-   **File Naming:** Files and folders generally follow `kebab-case` for directories and `PascalCase` for React components/screens (e.g., `ChatScreen.tsx`).
