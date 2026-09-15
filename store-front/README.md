# Store-Front

The store-front is the Vue.js frontend that allows users to select products and place orders.

## Requirements

- Node.js 24 LTS and npm (installed in the Order Service guide)
- Product and Order services running
- Start inside the repository's `store-front` directory. The main guide already takes you there.

## Setup Instructions

1. Install the versions recorded in the committed lockfile:

   ```bash
   npm ci
   ```

2. **Configure the API URLs before starting the Store Front.**

   For an Azure VM, open `src/components/OrderForm.vue` and replace the two URL strings in the `fetch(...)` calls:

   | Original URL                     | Replacement                           |
   | -------------------------------- | ------------------------------------- |
   | `http://localhost:3030/products` | `http://<VM-PUBLIC-IP>:3030/products` |
   | `http://localhost:3000/orders`   | `http://<VM-PUBLIC-IP>:3000/orders`   |

   Substitute your VM's actual public IP for `<VM-PUBLIC-IP>`. For a local installation, keep both localhost URLs.

   Your browser runs on your laptop. Its `localhost` points to that laptop; using the VM's public IP sends API requests to your VM. The development WebSocket automatically uses the address in your browser.

3. Start the Store Front:

   ```bash
   npm run serve
   ```

   Keep this terminal open; do not start a second copy from the main guide.

4. Open `http://<VM-PUBLIC-IP>:8080` for Azure, or `http://localhost:8080` locally. On Azure, use the public IP you saved in the portal, even if the terminal's Network URL shows a private IP.

Select one product, enter a positive quantity, and click **Place Order**. Two units of Dog Food should total **$39.98**. Verify the queued message using the RabbitMQ guide and check the browser console for errors.
