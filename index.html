<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Detector de Nariz y Música Doria Mejorado con Depuración</title>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/posenet"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        h1 { color: #333; }
        #videoContainer {
            position: relative;
            margin-bottom: 20px;
        }
        #video {
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .overlay {
            position: absolute;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 5px;
            border-radius: 5px;
            font-size: 14px;
        }
        #output { top: 10px; left: 10px; }
        #noteOutput { top: 10px; right: 10px; }
        #debugOutput { 
            bottom: 10px; 
            left: 10px; 
            max-height: 150px; 
            overflow-y: auto; 
            width: 300px;
        }
        #status {
            margin-top: 20px;
            font-weight: bold;
            text-align: center;
        }
        .note-section {
            position: absolute;
            left: 0;
            right: 0;
            height: 20%;
            border: 1px solid rgba(0, 0, 0, 0.2);
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 10px;
            color: black;
            font-weight: bold;
            transition: background-color 0.3s ease;
        }
    </style>
</head>
<body>
    <h1>Detector de Nariz y Música Doria Mejorado con Depuración</h1>
    <div id="videoContainer">
        <video id="video" width="640" height="480" autoplay></video>
        <div id="output" class="overlay"></div>
        <div id="noteOutput" class="overlay"></div>
        <div id="debugOutput" class="overlay"></div>
        <div id="laSection" class="note-section" style="top: 0;">La</div>
        <div id="solSection" class="note-section" style="top: 20%;">Sol</div>
        <div id="faSection" class="note-section" style="top: 40%;">Fa</div>
        <div id="miSection" class="note-section" style="top: 60%;">Mi</div>
        <div id="reSection" class="note-section" style="top: 80%;">Re</div>
    </div>
    <div id="status">Cargando modelo...</div>

    <script>
        const video = document.getElementById('video');
        const output = document.getElementById('output');
        const noteOutput = document.getElementById('noteOutput');
        const debugOutput = document.getElementById('debugOutput');
        const status = document.getElementById('status');
        const sections = ['laSection', 'solSection', 'faSection', 'miSection', 'reSection'];
        
        let model, midiAccess, midiOutput;
        let currentNote = null;
        let isDetecting = false;

        const notes = {
            'La': 69, 'Sol': 67, 'Fa': 65, 'Mi': 64, 'Re': 62
        };

        async function initMIDI() {
            try {
                midiAccess = await navigator.requestMIDIAccess();
                const outputs = Array.from(midiAccess.outputs.values());
                if (outputs.length > 0) {
                    midiOutput = outputs[0];
                    updateStatus('MIDI listo y modelo cargado. Detectando nariz...');
                } else {
                    throw new Error('No se encontró salida MIDI.');
                }
            } catch (error) {
                updateStatus(`Error MIDI: ${error.message}`);
            }
        }

        function sendMIDIMessage(noteNumber, velocity, type) {
            if (midiOutput) {
                midiOutput.send([type, noteNumber, velocity]);
            }
        }

        function playNoteMidi(noteNumber) {
            if (currentNote !== noteNumber) {
                stopNoteMidi(currentNote);
                sendMIDIMessage(noteNumber, 127, 0x90); // Note on
                updateNoteOutput(`Nota: ${Object.keys(notes).find(key => notes[key] === noteNumber)}`);
                currentNote = noteNumber;
                updateSectionColors(noteNumber);
            }
        }

        function stopNoteMidi(noteNumber) {
            if (noteNumber !== null) {
                sendMIDIMessage(noteNumber, 0, 0x80); // Note off
                updateNoteOutput('Nota: Ninguna');
                currentNote = null;
                updateSectionColors(null);
            }
        }

        function updateSectionColors(activeNote) {
            sections.forEach(section => {
                const sectionElement = document.getElementById(section);
                sectionElement.style.backgroundColor = section === `${activeNote}Section` 
                    ? 'rgba(0, 0, 0, 0.1)' 
                    : 'transparent';
            });
        }

        async function setupCamera() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: true });
                video.srcObject = stream;
                return new Promise((resolve) => {
                    video.onloadedmetadata = () => resolve(video);
                });
            } catch (error) {
                updateStatus(`Error de cámara: ${error.message}`);
                throw error;
            }
        }

        async function loadModel() {
            try {
                model = await posenet.load({
                    architecture: 'MobileNetV1',
                    outputStride: 16,
                    inputResolution: { width: 640, height: 480 },
                    multiplier: 0.75
                });
                updateStatus('Modelo cargado. Detectando nariz...');
            } catch (error) {
                updateStatus(`Error de modelo: ${error.message}`);
                throw error;
            }
        }

        async function detectPose() {
            if (!isDetecting) return;
            
            const pose = await model.estimateSinglePose(video, {
                flipHorizontal: false
            });
            
            updateDebugOutput(JSON.stringify(pose, null, 2));

            if (pose.score > 0.3) {  // Reducido el umbral de confianza general
                const nose = pose.keypoints.find(point => point.part === 'nose');
                if (nose && nose.score > 0.3) {  // Reducido el umbral de confianza para la nariz
                    const y = nose.position.y;
                    const section = Math.floor((y / video.height) * 5);
                    const noteName = Object.keys(notes)[section];
                    const midiNote = notes[noteName];
                    updateOutput(`Nariz detectada en sección: ${noteName} (Confianza: ${nose.score.toFixed(2)})`);
                    playNoteMidi(midiNote);
                } else {
                    handleNoseNotDetected(nose ? nose.score : 0);
                }
            } else {
                handleNoseNotDetected(pose.score);
            }

            requestAnimationFrame(detectPose);
        }

        function handleNoseNotDetected(score) {
            updateOutput(`Nariz no detectada claramente (Confianza: ${score.toFixed(2)})`);
            stopNoteMidi(currentNote);
        }

        function updateStatus(message) {
            status.textContent = message;
        }

        function updateOutput(message) {
            output.textContent = message;
        }

        function updateNoteOutput(message) {
            noteOutput.textContent = message;
        }

        function updateDebugOutput(message) {
            debugOutput.innerHTML = `<pre>${message}</pre>`;
        }

        async function init() {
            try {
                await setupCamera();
                await loadModel();
                await initMIDI();
                isDetecting = true;
                detectPose();
            } catch (error) {
                updateStatus(`Error de inicialización: ${error.message}`);
            }
        }

        init();
    </script>
</body>
</html>
