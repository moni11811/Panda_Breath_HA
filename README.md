## How to install

### A) Create the switch in Home Assistant (1 minute)

1. Go to: **Home Assistant → Settings → Devices & services → Helpers**
2. Click **Create Helper**
3. Choose **Toggle**
4. Name it: **Panda Breath Work**

Home Assistant will create an entity like:

* `input_boolean.panda_breath_work` (or similar)

You can click the helper to view its **Entity ID**.

---

### B) Build the Node-RED flow (exact nodes + what to fill)

Open Node-RED from:

* **Home Assistant → Settings → Add-ons → Node-RED → Open Web UI**

#### 1) Add node: `Events: state`

This node is from `node-red-contrib-home-assistant-websocket`.

1. Drag in **Events: state**
2. Double-click it
3. Set:

* **Entity ID:** `input_boolean.panda_breath_work`

(Leave the rest as defaults.)

This node fires whenever you toggle that helper in Home Assistant.

---

#### 2) Add node: `Function`

1. Drag in a **Function** node
2. Double-click it
3. Paste this code:

```js
// msg.payload will be "on" or "off" from Home Assistant
const isOn = (msg.payload === "on");

msg.payload = JSON.stringify({
  settings: { work_on: isOn }
});

return msg;
```

---

#### 3) Add node: `WebSocket out`

1. Drag in **WebSocket out**
2. Double-click it
3. For **Server**, click the pencil (edit)
4. Set **URL** to one of these:

* `ws://pandabreath.local/ws`
* `ws://IP/ws`

5. Click **Add/Update**
6. Click **Done**

---

#### 4) Wire them together

Connect:

* `Events: state` → `Function` → `WebSocket out`

---

#### 5) Deploy

Click **Deploy** (top right).
