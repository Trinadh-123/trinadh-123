<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DETECT THE BOOK Presentation (3D)</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Three.js library for 3D visualization -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        /* Custom styles for the 3D presentation theme */
        .slide-container {
            min-height: 100vh;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1f2937; /* Dark Slate Background */
            color: #e0f2fe; /* Light Blue/Cyan text */
        }
        #three-d-canvas {
            width: 100%;
            height: 500px; /* Fixed height for 3D area */
            background-color: #111827; /* Extra dark background for contrast */
            border-radius: 1rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
            margin-bottom: 1rem;
        }
        .main-content-wrapper {
            background-color: #374151; /* Darker gray card background */
            border: 2px solid #06b6d4;
        }
        .info-box {
            background-color: #06b6d4; /* Cyan */
            color: white;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
    </style>
</head>
<body>

    <div id="presentation-app" class="slide-container flex flex-col justify-between p-4 md:p-8">
        <!-- Slide Content Area -->
        <div id="slide-content" class="flex-grow flex items-center justify-center p-4">
            <!-- Content will be rendered here by JavaScript -->
        </div>

        <!-- Navigation Controls -->
        <div class="flex justify-between items-center p-4 bg-gray-700 shadow-lg rounded-xl mt-4 max-w-lg mx-auto w-full">
            <button onclick="changeSlide(-1)" id="prev-btn" class="px-4 py-2 bg-gray-600 text-white font-semibold rounded-full shadow-md transition duration-150 hover:bg-gray-500 disabled:opacity-50 disabled:cursor-not-allowed">
                &larr; Previous
            </button>
            <span id="slide-counter" class="text-sm font-medium text-gray-300">Slide X of 7</span>
            <button onclick="changeSlide(1)" id="next-btn" class="px-4 py-2 bg-blue-600 text-white font-semibold rounded-full shadow-md transition duration-150 hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed">
                Next &rarr;
            </button>
        </div>
    </div>

    <script>
        let currentSlide = 1;
        const totalSlides = 7;
        
        // Three.js variables
        let scene, camera, renderer, controls;
        let animationFrameId;
        let objects = [];
        let raycaster, mouse = new THREE.Vector2();
        let isDragging = false;
        let previousMousePosition = { x: 0, y: 0 };
        const canvasContainer = document.getElementById('slide-content');

        // --- Presentation Data ---
        const slides = [
            // Slide 1: Title Slide (2D)
            {
                type: 'title',
                title: "DETECT THE BOOK (The Library Mission)",
                subtitle: "Your Guide to Finding Any Book, Instantly.",
                class: 'bg-blue-800 text-white p-12 rounded-2xl shadow-2xl text-center'
            },
            // Slide 2: Introduction (2D)
            {
                type: 'list',
                title: "Mission Briefing: Why Are We Here?",
                items: [
                    "**The Problem:** Finding a specific book can feel like a search mission without a map.",
                    "**The Goal:** To turn confusing steps into a clear, three-part process.",
                    "**The Result:** You will leave here knowing how to find ANY book in the library in minutes!"
                ],
                class: 'main-content-wrapper p-8 rounded-xl shadow-lg'
            },
            // Slide 3: Key Factor 1 - Catalog Clues (3D)
            {
                type: 'three-d',
                mainTitle: "USES OF THIS APP (DETECT THE BOOK)",
                heading: "Key Factor 1: The Catalog Clues (Search Methods)",
                items: [
                    "**1. Core:** Time Saver (Instantly locate books, cutting search time).",
                    "**2. Core:** Find Location (Gives the exact shelf and section in the library).", // UPDATED CONTENT
                    "**3. Core:** Subject Search (Perfect for exploring a topic).",
                    "**4. Vision:** Image Recognition Search (Search by scanning a book cover).",
                    "**5. Vision:** Voice Command Search (Speak the title or subject query).",
                    "**6. Vision:** Semantic Topic Mapping (Search for concepts, not just keywords).",
                    "**7. Vision:** Multi-Language Metadata (Search across all languages).",
                    "**8. Vision:** Trend Analysis (See which books are currently popular in your library)."
                ]
            },
            // Slide 4: Key Factor 2 - The System's Mission (3D)
            {
                type: 'three-d',
                mainTitle: "USES OF THIS APP (DETECT THE BOOK)",
                heading: "Key Factor 2: The System's Mission (Features & Benefits)",
                items: [
                    "**1. Core:** Primary Uses (Designed for **Quick Search and Location**).",
                    "**2. Core:** Student Benefits (**Saves Time** and **Builds Confidence**).",
                    "**3. Core:** Origin of the Idea (Born from **Frustration to Solution**).",
                    "**4. Vision:** Real-Time Checkout Status (See if a book is currently available).",
                    "**5. Vision:** Digital Note Syncing (Integrate with study apps for research notes).",
                    "**6. Vision:** Collaborative Study Group Finder (Find peers studying the same book).",
                    "**7. Vision:** Personalized Reading Recommendations (AI-suggested reading list).",
                    "**8. Vision:** AI-Powered Research Assistant (Instant summary of key topics)."
                ]
            },
            // Slide 5: Key Factor 3 - Decoding the Numbers (3D)
            {
                type: 'three-d',
                mainTitle: "USES OF THIS APP (DETECT THE BOOK)",
                heading: "Key Factor 3: Decoding the Numbers (Location Tech)",
                items: [
                    "**1. Core:** The Numbers (Subject): Finds the correct shelf (e.g., 590s for animals).",
                    "**2. Core:** The Letters (Author): Finds the exact book on that shelf (usually author initials).",
                    "**3. Core:** Reading Order: Find the **shelf** (Numbers) FIRST, then the **book** (Letters).",
                    "**4. Vision:** AR Navigation Overlay (Use phone camera to see the book's location on shelves).",
                    "**5. Vision:** Integrated Floor Maps (2D map view with a blinking book icon).",
                    "**6. Vision:** Smart Shelf Lighting (System triggers a small light near the physical book).",
                    "**7. Vision:** Inventory Count Tracker (Confirms the number of copies on the shelf).",
                    "**8. Vision:** Reservation System Link (Hold a book instantly from the app)."
                ]
            },
            // Slide 6: Review, Q&A, and Vision (2D)
            {
                type: 'qa',
                title: "Mission Debrief: Questions and Ideas",
                questions: [
                    "Do you have any **doubts** about the 3-step process?",
                    "What is your **Vision** for the app? What features should we add next?"
                ],
                class: 'main-content-wrapper p-8 rounded-xl shadow-lg border-l-4 border-blue-600'
            },
            // Slide 7: Conclusion (2D)
            {
                type: 'thankyou',
                text: "THANK YOU",
                class: 'bg-blue-500 text-white p-20 rounded-2xl shadow-2xl text-center'
            }
        ];
        // --- End Presentation Data ---

        const slideContentEl = document.getElementById('slide-content');
        const slideCounterEl = document.getElementById('slide-counter');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');


        /** 3D Visualization Logic **/

        function init3D(slideData) {
            cleanup3D(); // Clear previous scene

            const container = document.createElement('div');
            container.id = 'three-d-canvas';
            container.classList.add('w-full', 'max-w-4xl');
            slideContentEl.appendChild(container);

            const width = container.clientWidth;
            const height = 500;

            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(width, height);
            container.appendChild(renderer.domElement);

            raycaster = new THREE.Raycaster();

            // Lighting: Subtle ambient and directional light
            scene.add(new THREE.AmbientLight(0x404040, 5));
            const directionalLight = new THREE.DirectionalLight(0xffffff, 3);
            directionalLight.position.set(1, 1, 1);
            scene.add(directionalLight);

            // Center Sphere (USES OF THIS APP (DETECT THE BOOK))
            const mainGeometry = new THREE.SphereGeometry(1.5, 32, 32);
            const mainMaterial = new THREE.MeshPhongMaterial({ color: 0x06b6d4, emissive: 0x06b6d4, emissiveIntensity: 0.5 });
            const mainSphere = new THREE.Mesh(mainGeometry, mainMaterial);
            scene.add(mainSphere);
            mainSphere.name = slideData.mainTitle;

            // --- 8 Orbiter Logic ---
            const contentGeometry = new THREE.SphereGeometry(0.5, 32, 32);
            const activeMaterial = new THREE.MeshPhongMaterial({ color: 0xe0f2fe, emissive: 0xe0f2fe, emissiveIntensity: 0.2 });
            const visionMaterial = new THREE.MeshPhongMaterial({ 
                color: 0x545c68, // Darker gray for future/vision points
                emissive: 0x545c68, 
                emissiveIntensity: 0.1,
                transparent: true,
                opacity: 0.6 // Slightly faded
            });

            // Positioning the 8 spheres around the center
            const orbitRadius = 3.5;
            const numOrbiters = 8;
            const contentCount = 3; // Only the first 3 items are the "Core" concepts

            objects = [mainSphere]; // Start with the main sphere

            for (let i = 0; i < numOrbiters; i++) {
                const angle = (i / numOrbiters) * Math.PI * 2;
                
                let sphere;
                let material;

                if (i < contentCount) {
                    // Core Content Sphere (First 3 points)
                    material = activeMaterial.clone();
                } else {
                    // Vision/Placeholder Sphere (Next 5 points)
                    material = visionMaterial.clone();
                }

                sphere = new THREE.Mesh(contentGeometry, material);
                sphere.userData.content = slideData.items[i]; // Store content for display
                sphere.name = `Point ${i + 1}`;
                
                // Set sphere position in a circle
                sphere.position.set(
                    orbitRadius * Math.cos(angle),
                    orbitRadius * Math.sin(angle),
                    0 // Keep it mostly 2D plane for clear visibility
                );
                
                scene.add(sphere);
                objects.push(sphere);
            }
            // --- End 8 Orbiter Logic ---

            camera.position.z = 7;

            // Add event listeners for camera control (mouse/touch drag)
            renderer.domElement.addEventListener('mousedown', onMouseDown, false);
            renderer.domElement.addEventListener('mousemove', onMouseMove, false);
            renderer.domElement.addEventListener('mouseup', onMouseUp, false);
            
            // Touch events for mobile
            renderer.domElement.addEventListener('touchstart', onTouchStart, false);
            renderer.domElement.addEventListener('touchmove', onTouchMove, false);
            renderer.domElement.addEventListener('touchend', onTouchEnd, false);

            window.addEventListener('resize', onWindowResize, false);
            
            // Initial render and start animation
            animate3D(slideData);
        }
        
        // Resizing the canvas
        function onWindowResize() {
            const container = document.getElementById('three-d-canvas');
            if (!container) return;
            const width = container.clientWidth;
            const height = 500;
            camera.aspect = width / height;
            camera.updateProjectionMatrix();
            renderer.setSize(width, height);
        }

        // Mouse/Touch controls for orbiting
        function onMouseDown(event) {
            isDragging = true;
            previousMousePosition.x = event.clientX;
            previousMousePosition.y = event.clientY;
        }

        function onTouchStart(event) {
            if (event.touches.length === 1) {
                isDragging = true;
                previousMousePosition.x = event.touches[0].clientX;
                previousMousePosition.y = event.touches[0].clientY;
            }
            event.preventDefault();
        }

        function onMouseMove(event) {
            if (!isDragging) return;
            
            const deltaX = event.clientX - previousMousePosition.x;
            const deltaY = event.clientY - previousMousePosition.y;

            // Rotate the entire scene group
            scene.rotation.y += deltaX * 0.005;
            scene.rotation.x += deltaY * 0.005;

            previousMousePosition.x = event.clientX;
            previousMousePosition.y = event.clientY;
        }

        function onTouchMove(event) {
            if (!isDragging || event.touches.length !== 1) return;
            
            const deltaX = event.touches[0].clientX - previousMousePosition.x;
            const deltaY = event.touches[0].clientY - previousMousePosition.y;

            scene.rotation.y += deltaX * 0.005;
            scene.rotation.x += deltaY * 0.005;

            previousMousePosition.x = event.touches[0].clientX;
            previousMousePosition.y = event.touches[0].clientY;
            event.preventDefault();
        }

        function onMouseUp() {
            isDragging = false;
        }

        function onTouchEnd() {
            isDragging = false;
        }
        
        // Hover/Touch to show content logic (using 2D overlay)
        function onHover(event, slideData) {
            // Calculate mouse position in normalized device coordinates (-1 to +1)
            const rect = renderer.domElement.getBoundingClientRect();
            mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1;
            mouse.y = -((event.clientY - rect.top) / rect.height) * 2 + 1;

            raycaster.setFromCamera(mouse, camera);
            const intersects = raycaster.intersectObjects(objects);

            const infoEl = document.getElementById('info-overlay');
            if (intersects.length > 0) {
                const intersectedObject = intersects[0].object;
                const content = intersectedObject.userData.content;

                if (content) {
                    // This is a sphere with actual content (Core or Vision)
                    infoEl.textContent = content;
                    infoEl.classList.remove('opacity-0');
                    infoEl.style.left = `${event.clientX - rect.left}px`;
                    infoEl.style.top = `${event.clientY - rect.top - 50}px`; // Position above cursor
                } else if (intersectedObject === objects[0]) {
                     // This is the main center sphere
                     infoEl.textContent = slideData.mainTitle;
                     infoEl.classList.remove('opacity-0');
                     infoEl.style.left = `${event.clientX - rect.left}px`;
                     infoEl.style.top = `${event.clientY - rect.top - 50}px`;
                }
            } else {
                infoEl.classList.add('opacity-0');
            }
        }

        function animate3D(slideData) {
            animationFrameId = requestAnimationFrame(() => animate3D(slideData));
            
            // Simple subtle rotation of content spheres (all orbiters: index 1 onwards)
            objects.slice(1).forEach(obj => {
                obj.rotation.x += 0.01;
                obj.rotation.y += 0.005;
            });
            
            // The main sphere pulses subtly
            objects[0].scale.set(
                1.5 + Math.sin(Date.now() * 0.001) * 0.1,
                1.5 + Math.sin(Date.now() * 0.001) * 0.1,
                1.5 + Math.sin(Date.now() * 0.001) * 0.1
            );
            
            renderer.render(scene, camera);
        }

        function cleanup3D() {
            if (animationFrameId) {
                cancelAnimationFrame(animationFrameId);
                animationFrameId = null;
            }
            if (renderer) {
                 renderer.domElement.removeEventListener('mousedown', onMouseDown, false);
                 renderer.domElement.removeEventListener('mousemove', onMouseMove, false);
                 renderer.domElement.removeEventListener('mouseup', onMouseUp, false);
                 renderer.domElement.removeEventListener('touchstart', onTouchStart, false);
                 renderer.domElement.removeEventListener('touchmove', onTouchMove, false);
                 renderer.domElement.removeEventListener('touchend', onTouchEnd, false);
                 renderer.domElement.removeEventListener('mousemove', onHover, false);
                 window.removeEventListener('resize', onWindowResize, false);
                 renderer.dispose();
            }
            scene = null;
            camera = null;
            renderer = null;
            objects = [];
            // Remove canvas and info overlay if they exist
            const oldCanvas = document.getElementById('three-d-canvas');
            if (oldCanvas) oldCanvas.remove();
            const oldInfo = document.getElementById('info-overlay');
            if (oldInfo) oldInfo.remove();
        }

        /** General Presentation Logic **/

        // Function to render the current slide based on its type
        function renderSlide(slideData) {
            cleanup3D(); // Always clean up 3D first in case we switch away from it
            slideContentEl.innerHTML = ''; // Clear previous content

            let html = '';

            if (slideData.type === 'three-d') {
                html = `
                    <div class="w-full max-w-4xl text-center p-4">
                        <h2 class="text-3xl font-bold text-blue-400 mb-6">${slideData.heading}</h2>
                        <div id="canvas-wrapper" class="relative">
                            <div id="info-overlay" class="absolute z-10 p-2 rounded-lg info-box font-bold pointer-events-none opacity-0 transition duration-300">
                                Hover over a sphere!
                            </div>
                        </div>
                        <p class="text-sm mt-4 text-gray-400">Drag your mouse or swipe the screen to view the 3D model!</p>
                    </div>
                `;
                slideContentEl.innerHTML = html;
                init3D(slideData);
                // Re-attach hover listener after canvas is initialized
                renderer.domElement.addEventListener('mousemove', (e) => onHover(e, slideData), false);

            } else {
                switch (slideData.type) {
                    case 'title':
                        html = `
                            <div class="max-w-4xl text-center ${slideData.class}">
                                <h1 class="text-4xl md:text-6xl font-extrabold tracking-tight mb-4">${slideData.title}</h1>
                                <p class="text-xl md:text-2xl font-light opacity-80">${slideData.subtitle}</p>
                            </div>
                        `;
                        break;

                    case 'list':
                        html = `
                            <div class="max-w-xl w-full text-gray-200 ${slideData.class}">
                                <h2 class="text-3xl font-bold text-blue-400 mb-6 border-b pb-2">${slideData.title}</h2>
                                <ul class="space-y-4 text-lg">
                                    ${slideData.items.map(item => `<li class="p-3 bg-gray-600 rounded-lg shadow-sm">${item}</li>`).join('')}
                                </ul>
                            </div>
                        `;
                        break;
                    
                    case 'qa':
                        html = `
                            <div class="max-w-xl w-full text-gray-200 ${slideData.class}">
                                <h2 class="text-3xl font-bold text-blue-400 mb-6">${slideData.title}</h2>
                                <div class="space-y-6 text-lg">
                                    ${slideData.questions.map((q, index) => `
                                        <div class="p-4 bg-gray-600 rounded-lg shadow-md border-l-4 border-yellow-500">
                                            <p class="font-semibold text-xl text-yellow-400">Q${index + 1}:</p>
                                            <p>${q}</p>
                                        </div>
                                    `).join('')}
                                </div>
                            </div>
                        `;
                        break;

                    case 'thankyou':
                        html = `
                            <div class="w-full max-w-4xl text-center ${slideData.class} transform scale-105
                            

