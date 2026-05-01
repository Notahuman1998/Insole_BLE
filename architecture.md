```mermaid
flowchart LR

%% HARDWARE LAYER
subgraph Hardware [Hardware Layer (ESP32 in Shoe)]
  direction TB
  F[Physical Force] -->|Crushes| FSR[8x FSR Array]
  M[Foot Movement] -->|Rotates| MPU[MPU6050 IMU]
  FSR -->|Analog Voltage| ADC[ESP32 ADC]
  MPU -->|I2C Data| I2C[ESP32 I2C Bus]
  ADC -->|16x Oversample & Trim| DSP[DSP Noise Filter]
  DSP -->|Resistance Ohms| MCU[ESP32 Main Loop]
  I2C -->|Pitch/Roll/Acc/Gyro| MCU
  MCU -->|Pack into Bytes| BLE_TX[BLE GATT Server]
end

%% COMMUNICATION LAYER
BLE_TX -->|100Hz Wireless Stream| BLE_RX[Web Bluetooth API]

%% SOFTWARE LAYER
subgraph Dashboard [Frontend Dashboard (Browser)]
  direction TB
  BLE_RX -->|Byte Array| Parser[Data Parser & Mapper]
  Parser -->|Parsed Variables| StateBuffer[State Buffers & Memory]
  StateBuffer -->|Pitch & Roll| ThreeJS[3D Render Engine]
  StateBuffer -->|Current FSR Ohms| MatrixUI[2D Pressure Matrix UI]
  StateBuffer -->|10-Packet Batches| PlotlyLive[Real-Time Line Chart]
  StateBuffer -->|300-Packet Window| StatsEngine[Math & Stats Engine]
  StateBuffer -->|Appended Strings| CSV[CSV Data Logger]
  CSV -->|Export| Download[Local .CSV File]
  Download -.->|Upload| Analyzer[Post-Process Analyzer]
end
```

Key points:

- The block must start with **three backticks**, then `mermaid`, on its own line.[page:1]  
- Remove the extra backticks and the surrounding inline backtick (`) that currently wrap the whole thing.[page:1]  
- Keep the diagram definition exactly as Mermaid syntax (no Markdown around it).[page:1]  

After you commit this change, GitHub should render the diagram visually above the code block in the file view.

## Use an external visual editor (optional)

If you want to experiment visually before committing, you can:

- Copy only the Mermaid diagram (everything from `flowchart LR` down, no backticks).[page:1]  
- Paste it into an online Mermaid editor like the Mermaid Live Editor or similar tools.  
- Tweak layout, labels, or directions there and then paste the final version back into your `architecture.md` inside a ` ```mermaid` code block.[page:1]  

Would you prefer to keep the entire architecture as one Mermaid flowchart, or split it into two diagrams (hardware vs. software) in the markdown file?  

<user_response_autocomplete>
Keep it as one flowchart in the file
I want two diagrams hardware and software
I also want a sequence diagram version
</user_response_autocomplete>
