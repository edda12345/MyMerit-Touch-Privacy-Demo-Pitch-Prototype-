MyMerit-Touch-Privacy-Demo (Pitch Prototype)
Interactive single-page React application designed to demonstrate the core technical architecture of the MyMerit Touch attendance system.
This prototype simulates a field device managing attendance logs and visually showcases three key innovations required for resilient, secure, and rapid deployment in the field.

🌟 Core Innovations Demonstrated
1. The Privacy Vault (Security)Feature: We do NOT store the raw IC number (e.g., 800101-10-5555).
Innovation: The app instantly converts the ID into a SHA-256 Hash salted with a unique event key.
Benefit: If the volunteer's phone is stolen, the data is mathematically useless to a thief.
Demonstrated on the dashboard by the instant hash conversion in the "Field Device" scanner.

2. Zero-Internet Core (Resilience)
Feature: Built on Android WorkManager + Encrypted SQLite architecture.
Innovation: The app is native-offline.It queues thousands of attendance logs and "silently syncs" in the background only when a stable connection is detected hours later.
Benefit: We can operate in the middle of a rainforest for weeks without a server connection, eliminating dependence on signal strength.
Demonstrated by toggling the "Network Status" to OFFLINE and observing the "Local Encrypted DB" queue rise.

3. Rapid-Fire Mode (Speed)Feature: Optimized APDU Command Set (SELECT -> READ BINARY).
Innovation: We stripped out all non-essential handshake protocols, achieving ultra-low latency.
Benefit: The 0.5-second scan time allows us to process a queue of 500 villagers in under 15 minutes. It's faster than a Touch 'n Go gate.
Demonstrated using the "Simulate 500 Villagers Queue" button, showing the high throughput in the Footer Stats.

🛠️ Technical Stack & SetupPlatform: React (built via Vite)
Styling: Tailwind CSS (Responsive and modern UI)
Icons: Lucide-React
Language: JavaScript (JSX)🚀 

How to Run Locally:
You must have Node.js installed on your system to run this application.
1. Clone the Repository:git clone [Your Repository URL]
cd MyMerit-Touch-Privacy-Demo
2. Install Dependencies:npm install
3. Start the Server:Note: Ensure you are in the MyMerit-Touch-Privacy-Demo directory.npm run dev
4. View the Demo: Open your browser and navigate to the address shown (typically http://localhost:5173/).
