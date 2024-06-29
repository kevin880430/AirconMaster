<!DOCTYPE html>
<html>
<html>
<head>
    <title>藍芽控制遊戲角色</title>
    <style>
        .button {
            width: 100px;
            height: 100px;
            margin: 10px;
            background-color: #ccc;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <h1>藍芽控制遊戲角色</h1>
    <button class="button" id="connectButton">連接藍芽</button>
    <div>
        <button class="button" id="upButton">上</button>
    </div>
    <div>
        <button class="button" id="leftButton">左</button>
        <button class="button" id="rightButton">右</button>
    </div>
    <div>
        <button class="button" id="downButton">下</button>
    </div>
    <button class="button" id="disconnectButton">解除連接藍芽</button>
    <script>
        // 藍芽連接狀態
        let bluetoothConnected = false;
        let bluetoothDevice;

        // 連接藍芽按鈕點擊事件處理函式
        document.getElementById("connectButton").addEventListener("click", async function() {
            if (!bluetoothConnected) {
                try {
                    // 請求使用者選擇藍芽設備
                    bluetoothDevice = await navigator.bluetooth.requestDevice({
                        filters: [{ services: ['generic_access'] }]
                    });

                    // 連接藍芽設備
                    await bluetoothDevice.gatt.connect();

                    console.log("藍芽已連接");
                    bluetoothConnected = true;
                    document.getElementById("connectButton").innerText = "解除藍芽";
                } catch (error) {
                    console.error("藍芽連接失敗:", error);
                }
            } else {
                // 解除藍芽連接
                if (bluetoothDevice && bluetoothDevice.gatt.connected) {
                    bluetoothDevice.gatt.disconnect();
                }

                console.log("藍芽已解除");
                bluetoothConnected = false;
                document.getElementById("connectButton").innerText = "連接藍芽";
            }
        });

        // 解除藍芽按鈕點擊事件處理函式
        document.getElementById("disconnectButton").addEventListener("click", function() {
            // 解除藍芽連接
            if (bluetoothDevice && bluetoothDevice.gatt.connected) {
                bluetoothDevice.gatt.disconnect();
            }

            console.log("藍芽已解除");
            bluetoothConnected = false;
            document.getElementById("connectButton").innerText = "連接藍芽";
        });
    </script>
</body>
</html>
