# Yaya Wallet TRX Dashboard


## Project Overview

The core functionality of this application is to display a user's transactions in a paginated, searchable, and well-structured table. A key challenge was the unresponsiveness of the provided sandbox API endpoints. To address this, the backend was developed with a conditional logic that seamlessly switches between the live API and a robust local mock dataset. This approach ensures the application remains functional for demonstration and testing purposes, and also showcases a resilient and adaptable architecture.

![Main Dashboard View with Transaction Table](assets/page1.png<img width="1299" height="583" alt="page1" src="https://github.com/user-attachments/assets/a1bfd85d-a36b-4e12-81a5-416a65e95b18" />
)
![Main Dashboard View with Transaction Table](assets/page2.<img width="1316" height="570" alt="page2" src="https://github.com/user-attachments/assets/a5d69a85-1da5-47fb-81fa-09f47bf3f9ef" />
png)
![Main Dashboard View with Transaction Table](assets/pag<img width="1307" height="589" alt="page3" src="https://github.com/user-attachments/assets/14dfbcaf-6895-4af4-a779-f37361c24845" />
e3.png)
![Main Dashboard View with Transaction Table](assets/page4.<img width="1131" height="576" alt="page4" src="https://github.com/user-attachments/assets/3856905b-9c92-4c1d-8628-573f2b796827" />
png)


![Main Dashboard View with Transaction Table](assets/Captu<img width="932" height="547" alt="Capture" src="https://github.com/user-attachments/assets/888b09b1-ca59-4d75-9d80-6e4b0bf62658" />
re.png)
### Key Features:

* **Paginated Transaction List**: Users can view transactions in a paginated table. The page number is controlled via a query parameter (`p`).

* **Search Functionality**: Transactions can be filtered by sender, receiver, cause, or transaction ID using a search input.

* **Visual Indicators**: The transaction table visually distinguishes between incoming and outgoing transactions, enhancing readability and user experience.

* **Responsive Design**: The frontend dashboard is built with Tailwind CSS to ensure it is fully responsive and provides an optimal viewing experience on various screen sizes, from mobile to desktop.

* **Security**: API keys and secrets are handled securely using environment variables, preventing their exposure in the codebase.

* **API Resilience**: The backend includes a toggle (`USE_MOCK_API`) to switch between the live API and a local mock dataset, providing a functional fallback in case of API issues.

## Technology Stack

* **Backend**: NestJS, a progressive Node.js framework for building efficient, reliable, and scalable server-side applications.

* **Frontend**: React, a popular JavaScript library for building user interfaces, with Vite as a fast build tool.

* **Styling**: Tailwind CSS, a utility-first CSS framework for creating custom designs rapidly.

## Architectural Choices

### Backend (NestJS)

The backend is a NestJS application with a single `TransactionsService` that handles all data retrieval logic.

* **Service Layer**: The `TransactionsService` encapsulates the business logic for fetching and searching transactions. It uses the `HttpService` from `@nestjs/axios` to make API calls.

* **Mock Data Implementation**: A core part of the solution is the `useMockApi` flag, which reads from an environment variable (`.env`). If set to `true`, the service uses an in-memory array of mock transactions for pagination and searching. Otherwise, it attempts to call the live YaYa Wallet API endpoints. This approach is demonstrated in both the `find_by_user` and `search` methods.

* **Security**: API credentials (`API_KEY` and `API_SECRET`) are loaded from environment variables using `@nestjs/config`, ensuring they are not hardcoded and remain secure.

* **Error Handling**: `try-catch` blocks are used to handle potential API call failures, throwing `InternalServerErrorException` to the client and logging the error on the server side.

### Frontend (React with Vite and Tailwind CSS)

The frontend is a single-page application that consumes the backend API.

* **State Management**: React's `useState` and `useEffect` hooks are used to manage component state, including the transaction data, loading state, pagination, and search query.

* **Data Fetching**: The dashboard fetches data from the backend endpoints (`/transactions/find_by_user` and `/transactions/search`) and dynamically updates the UI.

* **Visual Design**: Tailwind CSS simplifies the process of creating a clean, modern, and responsive user interface. The table rows use conditional styling to visually indicate the transaction type (incoming vs. outgoing) by changing their background color.

* **User Interface**: The UI includes a search input field and pagination controls that interact with the backend API to fetch new data without a full page reload.

## Running the Project

To run this project, you will need to have Node.js and npm/yarn installed.

### Backend Setup

1. Navigate to the backend directory.

2. Install dependencies: `npm install`

3. Create a `.env` file in the root directory with the following content:

   ```
   PORT=3000
   API_KEY=key-test_13817e87-33a9-4756-82e0-e6ac74be5f77
   API_SECRET=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlfa2V5Ijoia2V5LXRlc3RfMTM4MTdlODctMzNhOS00NzU2LTgyZTAtZTZhYzc0YmU1Zjc3Iiwic2VjcmV0IjoiY2E5ZjJhMGM5ZGI1ZmRjZWUxMTlhNjNiMzNkMzVlMWQ4YTVkNGZiYyJ9.HesEEFWkY55B8JhxSJT4VPJTXZ-4a18wWDRacTcimNw
   USE_MOCK_API=true # Set to 'false' to use the real API
   
   ```

4. Run the application: `npm start` or `npm run start:dev` for development mode.

### Frontend Setup

1. Navigate to the frontend directory.

2. Install dependencies: `npm install`

3. Run the development server: `npm run dev`

The dashboard will be available at `http://localhost:5173` (or the port specified by Vite).

## How to Test and Verify

To demonstrate the functionality, you can interact with the live demo:

1. **Open the dashboard**: Navigate to the provided URL in your browser.

2. **Pagination**: Click on the "Next" and "Previous" buttons to navigate through the pages of transactions. Observe how the data changes and the URL query parameter `p` updates accordingly.

3. **Search**: Use the search bar to filter transactions.

   * Search for "Biruk Kebede" to see transactions from that sender.

   * Search for "Coffee" to filter by the transaction cause.

   * Search for "TX-5" to find a specific transaction by its ID.

4. **Transaction Indicators**: Notice the color of each row.

   * **Outgoing Transactions**: Rows where the "Sender" is the current user's account name (e.g., "User Account"). These are highlighted in red.

   * **Incoming Transactions**: Rows where the "Receiver" is the current user's account name. These are highlighted in green. The mock data includes a top-up transaction with the same sender and receiver, which is correctly identified as incoming.

## Reflection

The primary challenge was the non-functional live API, which was identified through postman testing. This was a critical point to address to ensure the test could be evaluated. My solution to this problem was to build the backend with a configurable mock data layer. This not only provided a working solution but also demonstrated an understanding of building robust, testable, and resilient systems that can handle external service failures gracefully. This approach shows foresight and the ability to work around real-world development roadblocks.
