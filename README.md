<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, maximum-scale=1.0, minimum-scale=1.0">
    <title>مغامرة تلاغ في الليل - فرماجة يسكر بالراي</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #05050a;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        #canvas-container {
            width: 100%;
            height: 100%;
        }

        .modal-overlay {
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.95);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            padding: 20px;
        }

        .modal-card {
            background: linear-gradient(135deg, #111116, #1c1c24);
            border: 2px solid #ed1c24;
            border-radius: 15px;
            padding: 25px;
            max-width: 480px;
            width: 100%;
            text-align: center;
            color: #fff;
            box-shadow: 0 0 30px rgba(237, 28, 36, 0.4);
        }

        .modal-card h2 {
            color: #ffcc00;
            margin-top: 0;
            font-size: 22px;
        }

        .modal-card p {
            font-size: 15px;
            line-height: 1.6;
            margin: 15px 0;
            color: #ddd;
        }

        .btn-start {
            background: linear-gradient(135deg, #ed1c24, #b31217);
            color: white;
            border: none;
            padding: 14px 28px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 10px;
            cursor: pointer;
            width: 100%;
            box-shadow: 0 4px 12px rgba(0,0,0,0.5);
            margin-top: 10px;
        }

        #hud {
            position: absolute;
            top: 15px;
            right: 15px;
            left: 15px;
            background: rgba(10, 10, 15, 0.85);
            border: 1px solid #ffcc00;
            padding: 12px 16px;
            border-radius: 10px;
            color: white;
            font-size: 14px;
            z-index: 10;
            pointer-events: none;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }

        #quest-title {
            color: #ffcc00;
            font-weight: bold;
            margin-bottom: 4px;
        }

        #dialogue-box {
            position: absolute;
            bottom: 20px;
            left: 15px;
            right: 15px;
            background: rgba(15, 15, 22, 0.95);
            border: 2px solid #4bc0c0;
            border-radius: 12px;
            padding: 16px;
            color: white;
            display: none;
            z-index: 50;
            box-shadow: 0 0 20px rgba(0,0,0,0.8);
        }

        #speaker-name {
            color: #4bc0c0;
            font-weight: bold;
            font-size: 17px;
            margin-bottom: 8px;
        }

        #dialogue-text {
            font-size: 15px;
            line-height: 1.5;
        }

        #btn-dialogue-next {
            margin-top: 12px;
            background: #4bc0c0;
            color: #000;
            border: none;
            padding: 8px 18px;
            border-radius: 6px;
            font-weight: bold;
            float: left;
            cursor: pointer;
        }

        #touch-controls {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            pointer-events: none;
            z-index: 20;
        }

        #joystick-zone {
            position: absolute;
            bottom: 30px;
            left: 30px;
            width: 120px;
            height: 120px;
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid rgba(255, 255, 255, 0.25);
            border-radius: 50%;
            pointer-events: auto;
            touch-action: none;
        }

        #joystick-knob {
            position: absolute;
            top: 35px;
            left: 35px;
            width: 50px;
            height: 50px;
            background: rgba(237, 28, 36, 0.85);
            border-radius: 50%;
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
        }

        #camera-zone {
            position: absolute;
            top: 0;
            right: 0;
            width: 50vw;
            height: 100vh;
            pointer-events: auto;
            touch-action: none;
        }

        #btn-interact {
            position: absolute;
            bottom: 40px;
            right: 30px;
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, #ffcc00, #ff9900);
            border: 2px solid #ffffff;
            border-radius: 50%;
            color: #000;
            font-weight: bold;
            font-size: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            pointer-events: auto;
            box-shadow: 0 0 20px rgba(255, 204, 0, 0.5);
            display: none;
            z-index: 30;
        }

        #crosshair {
            position: absolute;
            top: 50%; left: 50%;
            width: 4px; height: 4px;
            background: rgba(255,255,255,0.6);
            border-radius: 50%;
            transform: translate(-50%, -50%);
            pointer-events: none;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>

    <div id="start-modal" class="modal-overlay">
        <div class="modal-card">
            <h2>🌙 مغامرة تلاغ في الليل!</h2>
            <p>لقد أضعت شريحة Ooredoo La Gold في شوارع تلاغ المظلمة. استخدم مصباحك اليدوي للبحث عنها واحذر من فرماجة السكران في أزقة المدينة!</p>
            <button class="btn-start" id="start-btn">ابدأ المغامرة 🔦</button>
        </div>
    </div>

    <div id="end-modal" class="modal-overlay" style="display: none;">
        <div class="modal-card" style="border-color: #ffcc00;">
            <h2>🎉 استرجعت الشريحة!</h2>
            <p id="end-text"></p>
            <button class="btn-start" onclick="location.reload()">إعادة اللعب 🔄</button>
        </div>
    </div>

    <div id="canvas-container"></div>
    <div id="crosshair"></div>

    <div id="hud">
        <div id="quest-title">📌 المهمة الحالية:</div>
        <div id="quest-desc">ابحث عن كشك جلول الفليكساتور واسأله عن الشريحة.</div>
    </div>

    <div id="touch-controls">
        <div id="joystick-zone">
            <div id="joystick-knob"></div>
        </div>
        <div id="camera-zone"></div>
        <div id="btn-interact" onclick="handleInteraction()">💬 تكلم</div>
    </div>

    <div id="dialogue-box">
        <div id="speaker-name">الشخصية</div>
        <div id="dialogue-text">نص الحوار...</div>
        <button id="btn-dialogue-next" onclick="closeDialogue()">متابعة ↵</button>
    </div>

    <script>
        let scene, camera, renderer;
        let player, flashlight, mouseLook = { x: 0, y: 0 };
        let isGameStarted = false;

        let currentQuestStep = 0;
        let activeNPC = null;
        let isDialogueOpen = false;
        let npcs = [];
        let audioCtx = null;

        let moveInput = { x: 0, y: 0 };
        let joystickTouchId = null;
        let cameraTouchId = null;
        let lastTouchPos = { x: 0, y: 0 };

        function init3D() {
            const container = document.getElementById('canvas-container');

            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x020208);
            scene.fog = new THREE.FogExp2(0x020208, 0.018);

            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            renderer.shadowMap.enabled = true;
            container.appendChild(renderer.domElement);

            const nightLight = new THREE.AmbientLight(0x1a233a, 0.3);
            scene.add(nightLight);

            const moonLight = new THREE.DirectionalLight(0x4a6572, 0.4);
            moonLight.position.set(50, 100, 50);
            scene.add(moonLight);

            const floorGeo = new THREE.PlaneGeometry(300, 300);
            const floorMat = new THREE.MeshStandardMaterial({ color: 0x111315, roughness: 0.8 });
            const floor = new THREE.Mesh(floorGeo, floorMat);
            floor.rotation.x = -Math.PI / 2;
            scene.add(floor);

            createRoadNetwork();

            player = new THREE.Group();
            player.position.set(0, 1.6, 70);
            scene.add(player);
            player.add(camera);

            flashlight = new THREE.SpotLight(0xfffaed, 2.5, 40, Math.PI / 6, 0.4, 1);
            flashlight.position.set(0, 0, 0);
            flashlight.target.position.set(0, 0, -1);
            camera.add(flashlight);
            camera.add(flashlight.target);

            buildCity();
            setupTouchEvents();
            window.addEventListener('resize', onWindowResize);
        }

        function createRoadNetwork() {
            const roadGeo = new THREE.PlaneGeometry(24, 300);
            const roadMat = new THREE.MeshStandardMaterial({ color: 0x1f1f24, roughness: 0.9 });
            const mainRoad = new THREE.Mesh(roadGeo, roadMat);
            mainRoad.rotation.x = -Math.PI / 2;
            mainRoad.position.y = 0.02;
            scene.add(mainRoad);

            const sidewalkGeo = new THREE.BoxGeometry(4, 0.2, 300);
            const sidewalkMat = new THREE.MeshStandardMaterial({ color: 0x3a3a40 });
            
            const leftSidewalk = new THREE.Mesh(sidewalkGeo, sidewalkMat);
            leftSidewalk.position.set(-14, 0.1, 0);
            scene.add(leftSidewalk);

            const rightSidewalk = new THREE.Mesh(sidewalkGeo, sidewalkMat);
            rightSidewalk.position.set(14, 0.1, 0);
            scene.add(rightSidewalk);
        }

        function buildCity() {
            createKiosk(-18, 0, 30, 0xd32f2f, "كشك جلول الفليكساتور", "djalil");
            createKiosk(18, 0, -20, 0x1976d2, "كشك يوسف كوسميتيك", "youssef");
            createFarjadNPC(0, 0, -90);

            for (let z = -120; z <= 100; z += 35) {
                if (Math.abs(z - 30) > 15 && Math.abs(z + 20) > 15) {
                    createHouse(-30, 0, z, 20, 12, 20, 0x424242);
                    createHouse(30, 0, z, 20, 14, 20, 0x37474f);
                }
            }

            for (let z = -110; z <= 90; z += 30) {
                createStreetLight(-13, z);
                createStreetLight(13, z);
            }
        }

        function createStreetLight(x, z) {
            const group = new THREE.Group();
            group.position.set(x, 0, z);

            const poleGeo = new THREE.CylinderGeometry(0.15, 0.2, 7);
            const poleMat = new THREE.MeshStandardMaterial({ color: 0x222222, metalness: 0.8 });
            const pole = new THREE.Mesh(poleGeo, poleMat);
            pole.position.y = 3.5;
            group.add(pole);

            const light = new THREE.PointLight(0xffaa44, 1.2, 18);
            light.position.set(0, 6.8, 0);
            group.add(light);

            const bulbGeo = new THREE.SphereGeometry(0.3);
            const bulbMat = new THREE.MeshBasicMaterial({ color: 0xffddaa });
            const bulb = new THREE.Mesh(bulbGeo, bulbMat);
            bulb.position.set(0, 6.8, 0);
            group.add(bulb);

            scene.add(group);
        }

        function createHouse(x, y, z, w, h, d, color) {
            const group = new THREE.Group();
            group.position.set(x, y, z);

            const wallGeo = new THREE.BoxGeometry(w, h, d);
            const wallMat = new THREE.MeshStandardMaterial({ color: color, roughness: 0.7 });
            const house = new THREE.Mesh(wallGeo, wallMat);
            house.position.y = h / 2;
            group.add(house);

            const winMat = new THREE.MeshBasicMaterial({ color: 0xffea8a });
            for (let fy = 3; fy < h - 2; fy += 4) {
                for (let fx = -w/3; fx <= w/3; fx += w/2) {
                    const winGeo = new THREE.PlaneGeometry(1.5, 2);
                    const win = new THREE.Mesh(winGeo, winMat);
                    win.position.set(fx, fy, (d/2) + 0.05);
                    group.add(win);
                }
            }

            scene.add(group);
        }

        function createKiosk(x, y, z, color, name, id) {
            const group = new THREE.Group();
            group.position.set(x, y, z);

            const bodyGeo = new THREE.BoxGeometry(7, 4.5, 5);
            const bodyMat = new THREE.MeshStandardMaterial({ color: color });
            const body = new THREE.Mesh(bodyGeo, bodyMat);
            body.position.y = 2.25;
            group.add(body);

            const signGeo = new THREE.BoxGeometry(6, 1, 0.2);
            const signMat = new THREE.MeshBasicMaterial({ color: 0xffffff });
            const sign = new THREE.Mesh(signGeo, signMat);
            sign.position.set(0, 4, 2.6);
            group.add(sign);

            const light = new THREE.PointLight(0xffffff, 1, 8);
            light.position.set(0, 3, 3);
            group.add(light);

            const npcGeo = new THREE.CylinderGeometry(0.5, 0.5, 1.8);
            const npcMat = new THREE.MeshStandardMaterial({ color: 0xffdbac });
            const npc = new THREE.Mesh(npcGeo, npcMat);
            npc.position.set(0, 1, 3);
            group.add(npc);

            scene.add(group);
            npcs.push({ id: id, name: name, x: x, z: z + 3 });
        }

        function createFarjadNPC(x, y, z) {
            const group = new THREE.Group();
            group.position.set(x, y, z);

            const bodyGeo = new THREE.CylinderGeometry(0.6, 0.6, 1.8);
            const bodyMat = new THREE.MeshStandardMaterial({ color: 0x880000 });
            const body = new THREE.Mesh(bodyGeo, bodyMat);
            body.position.y = 0.9;
            group.add(body);

            const headGeo = new THREE.SphereGeometry(0.4);
            const headMat = new THREE.MeshStandardMaterial({ color: 0xffdbac });
            const head = new THREE.Mesh(headGeo, headMat);
            head.position.y = 2;
            group.add(head);

            const bottleGeo = new THREE.CylinderGeometry(0.1, 0.1, 0.6);
            const bottleMat = new THREE.MeshStandardMaterial({ color: 0x00ff00, roughness: 0.1 });
            const bottle = new THREE.Mesh(bottleGeo, bottleMat);
            bottle.position.set(0.7, 0.3, 0);
            group.add(bottle);

            const redLight = new THREE.PointLight(0xff0000, 1, 5);
            redLight.position.set(0, 2, 0);
            group.add(redLight);

            scene.add(group);
            npcs.push({ id: "farmaja", name: "فرماجة السكران", x: x, z: z });
        }

        function setupTouchEvents() {
            const joystickZone = document.getElementById('joystick-zone');
            const joystickKnob = document.getElementById('joystick-knob');
            const cameraZone = document.getElementById('camera-zone');

            joystickZone.addEventListener('touchstart', (e) => {
                const touch = e.changedTouches[0];
                joystickTouchId = touch.identifier;
                updateJoystick(touch, joystickZone, joystickKnob);
            });

            joystickZone.addEventListener('touchmove', (e) => {
                for (let touch of e.changedTouches) {
                    if (touch.identifier === joystickTouchId) {
                        updateJoystick(touch, joystickZone, joystickKnob);
                    }
                }
            });

            const resetJoystick = (e) => {
                for (let touch of e.changedTouches) {
                    if (touch.identifier === joystickTouchId) {
                        joystickTouchId = null;
                        moveInput = { x: 0, y: 0 };
                        joystickKnob.style.top = '35px';
                        joystickKnob.style.left = '35px';
                    }
                }
            };

            joystickZone.addEventListener('touchend', resetJoystick);
            joystickZone.addEventListener('touchcancel', resetJoystick);

            cameraZone.addEventListener('touchstart', (e) => {
                const touch = e.changedTouches[0];
                cameraTouchId = touch.identifier;
                lastTouchPos = { x: touch.clientX, y: touch.clientY };
            });

            cameraZone.addEventListener('touchmove', (e) => {
                if (isDialogueOpen) return;
                for (let touch of e.changedTouches) {
                    if (touch.identifier === cameraTouchId) {
                        const deltaX = touch.clientX - lastTouchPos.x;
                        const deltaY = touch.clientY - lastTouchPos.y;

                        player.rotation.y -= deltaX * 0.005;
                        mouseLook.y -= deltaY * 0.005;
                        mouseLook.y = Math.max(-Math.PI / 3, Math.min(Math.PI / 3, mouseLook.y));
                        camera.rotation.x = mouseLook.y;

                        lastTouchPos = { x: touch.clientX, y: touch.clientY };
                    }
                }
            });

            const resetCamera = (e) => {
                for (let touch of e.changedTouches) {
                    if (touch.identifier === cameraTouchId) {
                        cameraTouchId = null;
                    }
                }
            };

            cameraZone.addEventListener('touchend', resetCamera);
            cameraZone.addEventListener('touchcancel', resetCamera);
        }

        function updateJoystick(touch, zone, knob) {
            const rect = zone.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;

            let deltaX = touch.clientX - centerX;
            let deltaY = touch.clientY - centerY;
            const maxDist = 45;

            const dist = Math.hypot(deltaX, deltaY);
            if (dist > maxDist) {
                deltaX = (deltaX / dist) * maxDist;
                deltaY = (deltaY / dist) * maxDist;
            }

            knob.style.left = `${35 + deltaX}px`;
            knob.style.top = `${35 + deltaY}px`;

            moveInput.x = deltaX / maxDist;
            moveInput.y = deltaY / maxDist;
        }

        // توليد لحن الراي برمجياً بدون الحاجة لملفات صوت خارجية
        function playSynthesizedRaiMusic() {
            try {
                if (!audioCtx) {
                    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                }
                if (audioCtx.state === 'suspended') {
                    audioCtx.resume();
                }

                const notes = [293.66, 329.63, 349.23, 392.00, 440.00, 466.16, 523.25];
                let step = 0;

                const interval = setInterval(() => {
                    if (!isGameStarted) {
                        clearInterval(interval);
                        return;
                    }
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();
                    
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(notes[step % notes.length], audioCtx.currentTime);
                    
                    gain.gain.setValueAtTime(0.15, audioCtx.curre
