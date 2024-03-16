# React Query

## Setting UP react query

### Installation
```shell
npm i @tanstack/react-query
```

### Setting up Query Provider

```js
// main.jsx

import React from "react";
import ReactDOM from "react-dom/client";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";  // imported

import App from "./App.jsx";

const queryClient = new QueryClient();    // created a client

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>    // used the client by wrapping it around the app component
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

### Adding JS code for handling query and api

```js
// useSellers.js

import { useQuery } from "@tanstack/react-query";
import apiClient from "../../utils/api-client";

const useGetSellers = (param) => {
  console.log(param);
  return useQuery({
    queryKey: ["sellers"],
    queryFn: () => apiClient.get("/users").then((res) => res.data),
  });
};

export { useGetSellers };
```

### Importing the code to component

```js
// Sellers.jsx
import React, { useEffect, useState } from "react";
import apiClient from "../../utils/api-client";
import Loader from "../Common/Loader";
import { useGetSellers } from "../api/useSellers";

const Sellers = () => {
  const [name, setName] = useState("");
  const sellers = useGetSellers({ params: "some params" });

  return (
    <>
      <h3>Admin Sellers Page</h3>
      <input type="text" onChange={(e) => setName(e.target.value)} />
      <button onClick={addSeller}>Add Seller</button>
      <div>
        {sellers.isLoading && <Loader />}
        {sellers.error && <em>{sellers.error.message}</em>}
      </div>

      <table>
        <tbody>
          {sellers.data?.map((seller) => (
            <tr key={seller.id}>
              <td>{seller.name}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </>
  );
};

export default Sellers;
```
