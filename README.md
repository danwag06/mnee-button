# MNEE Button

A lightweight snippet that embeds a “Pay Now” style MNEE button into your application or website. When clicked, it connects to Yours wallet, processes a MNEE payment, and shows a success message. The button is hosted on chain and can be imported as a script.

## Buy Me a Coffee ☕️

<a href="https://ordfs.network/content/003fdb9393b5035655b3e6b94ffcc414ec524d71bcaeaf128eedb9dd8aa8d178_0" target="_blank">
    <button style="background-color: #08121E; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">Buy Coffee - Example</button>
</a>

## Quick Start - HTML

1. Include it in your HTML with the desired attributes:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>MNEE Button</title>
  </head>
  <body>
    <script>
      function onPaymentSuccess(txid) {
        console.log(`Payment success triggered! 🎉\nTXID: ${txid}`);
      }

      function onPaymentError() {
        console.log("Payment error triggered!");
      }
    </script>
    <script
      src="https://ordfs.network/content/e6eb85b2eafb52d76c251a69905323bb3a2acb55eea872a62f785598125521d2_0"
      data-mnee-amount="1.50"
      data-mnee-address="15mNxEkyKJXPD8amic6oLUjS45zBKQQoLu"
      data-mnee-color="#08121E"
      data-mnee-label="Pay Now"
      data-mnee-success="onPaymentSuccess"
      data-mnee-error="onPaymentError"
    ></script>
  </body>
</html>
```

## Quick Start - React Application

1. Create a MneeButton.tsx component

```jsx
import { useEffect, useRef } from "react";

interface MneeButtonProps {
  amount: number;
  address: string;
  color?: string;
  label?: string;
  onSuccess?: () => void;
  onError?: () => void;
}

const MneeButton = ({
  amount,
  address,
  color = "#08121E",
  label = "Pay Now",
  onSuccess,
  onError,
}: MneeButtonProps) => {
  const buttonContainerRef = useRef<HTMLDivElement | null>(null);
  const scriptId = "mnee-payment-script";

  useEffect(() => {
    // Define callback functions globally
    (window as any).onMneePaymentSuccess = () => {
      if (onSuccess) onSuccess();
    };

    (window as any).onMneePaymentError = () => {
      if (onError) onError();
    };

    // Ensure the container exists before adding the script
    if (buttonContainerRef.current) {
      // Remove existing script if present
      const existingScript = document.getElementById(scriptId);
      if (existingScript) {
        document.body.removeChild(existingScript);
      }

      // Create the script element dynamically
      const script = document.createElement("script");
      script.src =
        "https://ordfs.network/content/e6eb85b2eafb52d76c251a69905323bb3a2acb55eea872a62f785598125521d2_0";
      script.async = true;
      script.id = scriptId;
      script.setAttribute("data-mnee-amount", amount.toString());
      script.setAttribute("data-mnee-address", address);
      script.setAttribute("data-mnee-color", color);
      script.setAttribute("data-mnee-label", label);
      script.setAttribute("data-mnee-success", "onMneePaymentSuccess");
      script.setAttribute("data-mnee-error", "onMneePaymentError");

      // Wait for React to fully render before appending the script
      setTimeout(() => {
        document.body.appendChild(script);
      }, 100);
    }

    return () => {
      // Cleanup script when unmounting
      const cleanupScript = document.getElementById(scriptId);
      if (cleanupScript) {
        document.body.removeChild(cleanupScript);
      }
    };
  }, [amount, address, color, label, onSuccess, onError]);

  return <div ref={buttonContainerRef} id="mnee-button-container"></div>;
};

export default MneeButton;
```

2. Use the component in your application

```jsx
import MneeButton from "./MneeButton";

const App = () => {
  const handleSuccess = () => {
    console.log("Payment was successful!");
  };

  const handleError = () => {
    console.log("Payment failed!");
  };

  return (
    <div>
      <h1>MNEE Payment Button</h1>
      <MneeButton
        amount={1.5}
        address="15mNxEkyKJXPD8amic6oLUjS45zBKQQoLu"
        color="#08121E"
        label="Pay Now"
        onSuccess={handleSuccess}
        onError={handleError}
      />
    </div>
  );
};

export default App;
```

## License

MIT – free for any personal or commercial use.
