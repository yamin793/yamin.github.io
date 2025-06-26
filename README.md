<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom QR Code Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.1/build/qrcode.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .qr-eye-preview {
            width: 60px;
            height: 60px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
        }
        .qr-eye-preview:hover {
            transform: scale(1.05);
            box-shadow: 0 0 10px rgba(0,0,0,0.2);
        }
        .qr-eye-preview.active {
            border: 3px solid #3b82f6;
            box-shadow: 0 0 15px rgba(59, 130, 246, 0.5);
        }
        #qrCodeContainer {
            position: relative;
            display: inline-block;
        }
        .qr-overlay {
            position: absolute;
            pointer-events: none;
        }
        .download-btn {
            transition: all 0.3s;
        }
        .download-btn:hover {
            transform: translateY(-2px);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">
    <div class="container mx-auto px-4 py-8">
        <div class="max-w-4xl mx-auto bg-white rounded-xl shadow-md overflow-hidden p-6">
            <h1 class="text-3xl font-bold text-center text-gray-800 mb-2">Custom QR Code Generator</h1>
            <p class="text-center text-gray-600 mb-8">Create beautiful, customizable QR codes with unique eye styles</p>
            
            <div class="flex flex-col lg:flex-row gap-8">
                <!-- Left side - Controls -->
                <div class="w-full lg:w-1/2 space-y-6">
                    <div>
                        <label for="qrContent" class="block text-sm font-medium text-gray-700 mb-1">QR Code Content</label>
                        <input type="text" id="qrContent" placeholder="Enter URL or text" 
                               class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500" 
                               value="https://example.com">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">QR Code Size</label>
                        <input type="range" id="qrSize" min="100" max="500" value="200" 
                               class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                        <div class="flex justify-between text-xs text-gray-500 mt-1">
                            <span>Small</span>
                            <span>Medium</span>
                            <span>Large</span>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">QR Code Color</label>
                        <div class="flex items-center space-x-4">
                            <input type="color" id="qrColor" value="#000000" class="w-12 h-12 cursor-pointer">
                            <input type="color" id="qrBgColor" value="#ffffff" class="w-12 h-12 cursor-pointer">
                            <div>
                                <span class="block text-xs">Foreground</span>
                                <span class="block text-xs">Background</span>
                            </div>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Eye Style</label>
                        <div class="grid grid-cols-3 gap-3">
                            <div class="qr-eye-preview active" data-eye-style="square" style="background-color: #000000;">
                                <div class="w-full h-full flex items-center justify-center text-white">
                                    <i class="fas fa-square-full text-3xl"></i>
                                </div>
                            </div>
                            <div class="qr-eye-preview" data-eye-style="circle" style="background-color: #000000;">
                                <div class="w-full h-full flex items-center justify-center text-white">
                                    <i class="fas fa-circle text-3xl"></i>
                                </div>
                            </div>
                            <div class="qr-eye-preview" data-eye-style="rounded" style="background-color: #000000;">
                                <div class="w-full h-full flex items-center justify-center text-white">
                                    <i class="fas fa-square text-3xl" style="border-radius: 20%;"></i>
                                </div>
                            </div>
                            <div class="qr-eye-preview" data-eye-style="dot" style="background-color: #000000;">
                                <div class="w-full h-full flex items-center justify-center text-white">
                                    <i class="fas fa-dot-circle text-3xl"></i>
                                </div>
                            </div>
                            <div class="qr-eye-preview" data-eye-style="diamond" style="background-color: #000000;">
                                <div class="w-full h-full flex items-center justify-center text-white">
                                    <i class="fas fa-gem text-3xl"></i>
                                </div>
                            </div>
                            <div class="qr-eye-preview" data-eye-style="heart" style="background-color: #000000;">
                                <div class="w-full h-full flex items-center justify-center text-white">
                                    <i class="fas fa-heart text-3xl"></i>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Eye Color</label>
                        <div class="flex items-center space-x-4">
                            <input type="color" id="eyeColor" value="#000000" class="w-12 h-12 cursor-pointer">
                            <input type="color" id="eyeInnerColor" value="#000000" class="w-12 h-12 cursor-pointer">
                            <div>
                                <span class="block text-xs">Outer</span>
                                <span class="block text-xs">Inner</span>
                            </div>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Options</label>
                        <div class="space-y-2">
                            <div class="flex items-center">
                                <input type="checkbox" id="addLogo" class="w-4 h-4 text-blue-600 rounded focus:ring-blue-500">
                                <label for="addLogo" class="ml-2 text-sm text-gray-700">Add Center Logo</label>
                            </div>
                            <div class="flex items-center">
                                <input type="checkbox" id="addText" class="w-4 h-4 text-blue-600 rounded focus:ring-blue-500">
                                <label for="addText" class="ml-2 text-sm text-gray-700">Add Text Below</label>
                            </div>
                        </div>
                    </div>
                    
                    <button id="generateBtn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-lg transition duration-200">
                        Generate QR Code
                    </button>
                </div>
                
                <!-- Right side - Preview -->
                <div class="w-full lg:w-1/2 flex flex-col items-center justify-center space-y-6">
                    <div id="qrCodeContainer" class="bg-white p-4 rounded-lg shadow-md">
                        <!-- QR code will be generated here -->
                    </div>
                    
                    <div class="flex space-x-4">
                        <button id="downloadPNG" class="download-btn bg-green-600 hover:bg-green-700 text-white font-medium py-2 px-4 rounded-lg flex items-center">
                            <i class="fas fa-download mr-2"></i> PNG
                        </button>
                        <button id="downloadSVG" class="download-btn bg-purple-600 hover:bg-purple-700 text-white font-medium py-2 px-4 rounded-lg flex items-center">
                            <i class="fas fa-download mr-2"></i> SVG
                        </button>
                        <button id="downloadJPG" class="download-btn bg-yellow-600 hover:bg-yellow-700 text-white font-medium py-2 px-4 rounded-lg flex items-center">
                            <i class="fas fa-download mr-2"></i> JPG
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Elements
            const qrContent = document.getElementById('qrContent');
            const qrSize = document.getElementById('qrSize');
            const qrColor = document.getElementById('qrColor');
            const qrBgColor = document.getElementById('qrBgColor');
            const eyeColor = document.getElementById('eyeColor');
            const eyeInnerColor = document.getElementById('eyeInnerColor');
            const addLogo = document.getElementById('addLogo');
            const addText = document.getElementById('addText');
            const generateBtn = document.getElementById('generateBtn');
            const qrCodeContainer = document.getElementById('qrCodeContainer');
            const downloadPNG = document.getElementById('downloadPNG');
            const downloadSVG = document.getElementById('downloadSVG');
            const downloadJPG = document.getElementById('downloadJPG');
            const eyePreviews = document.querySelectorAll('.qr-eye-preview');
            
            // Variables
            let currentEyeStyle = 'square';
            
            // Event Listeners
            eyePreviews.forEach(preview => {
                preview.addEventListener('click', function() {
                    eyePreviews.forEach(p => p.classList.remove('active'));
                    this.classList.add('active');
                    currentEyeStyle = this.getAttribute('data-eye-style');
                    generateQRCode();
                });
            });
            
            [qrContent, qrSize, qrColor, qrBgColor, eyeColor, eyeInnerColor, addLogo, addText].forEach(element => {
                element.addEventListener('input', generateQRCode);
                element.addEventListener('change', generateQRCode);
            });
            
            generateBtn.addEventListener('click', generateQRCode);
            downloadPNG.addEventListener('click', downloadQRCode.bind(null, 'png'));
            downloadSVG.addEventListener('click', downloadQRCode.bind(null, 'svg'));
            downloadJPG.addEventListener('click', downloadQRCode.bind(null, 'jpg'));
            
            // Initial generation
            generateQRCode();
            
            // Functions
            function generateQRCode() {
                const content = qrContent.value || 'https://example.com';
                const size = parseInt(qrSize.value);
                const color = qrColor.value;
                const bgColor = qrBgColor.value;
                const eyeOuterColor = eyeColor.value;
                const eyeInnerCol = eyeInnerColor.value;
                const hasLogo = addLogo.checked;
                const hasText = addText.checked;
                
                // Clear previous QR code
                qrCodeContainer.innerHTML = '';
                
                // Generate basic QR code
                QRCode.toCanvas(content, {
                    width: size,
                    color: {
                        dark: color,
                        light: bgColor
                    },
                    margin: 1
                }, function(err, canvas) {
                    if (err) {
                        console.error(err);
                        return;
                    }
                    
                    // Add canvas to container
                    qrCodeContainer.appendChild(canvas);
                    
                    // Add eye styles
                    applyEyeStyles(canvas, currentEyeStyle, eyeOuterColor, eyeInnerCol);
                    
                    // Add logo if needed
                    if (hasLogo) {
                        addCenterLogo(canvas);
                    }
                    
                    // Add text if needed
                    if (hasText) {
                        addTextBelow(canvas, content);
                    }
                });
            }
            
            function applyEyeStyles(canvas, style, outerColor, innerColor) {
                const ctx = canvas.getContext('2d');
                const size = canvas.width;
                const moduleSize = size / 21; // QR code has 21 modules per side
                const eyeSize = 7 * moduleSize;
                
                // Positions of the three eye patterns in a QR code
                const eyePositions = [
                    {x: 0, y: 0}, // Top-left
                    {x: size - eyeSize, y: 0}, // Top-right
                    {x: 0, y: size - eyeSize} // Bottom-left
                ];
                
                eyePositions.forEach(pos => {
                    // Draw outer eye
                    ctx.fillStyle = outerColor;
                    
                    switch(style) {
                        case 'circle':
                            ctx.beginPath();
                            ctx.arc(pos.x + eyeSize/2, pos.y + eyeSize/2, eyeSize/2, 0, Math.PI * 2);
                            ctx.fill();
                            break;
                        case 'rounded':
                            const cornerRadius = eyeSize * 0.2;
                            roundedRect(ctx, pos.x, pos.y, eyeSize, eyeSize, cornerRadius);
                            ctx.fill();
                            break;
                        case 'dot':
                            ctx.beginPath();
                            ctx.arc(pos.x + eyeSize/2, pos.y + eyeSize/2, eyeSize/2, 0, Math.PI * 2);
                            ctx.fill();
                            
                            // Inner dot
                            ctx.fillStyle = innerColor;
                            ctx.beginPath();
                            ctx.arc(pos.x + eyeSize/2, pos.y + eyeSize/2, eyeSize/4, 0, Math.PI * 2);
                            ctx.fill();
                            break;
                        case 'diamond':
                            diamondShape(ctx, pos.x, pos.y, eyeSize, eyeSize);
                            ctx.fill();
                            break;
                        case 'heart':
                            drawHeart(ctx, pos.x + eyeSize/2, pos.y + eyeSize/2, eyeSize/2);
                            ctx.fill();
                            break;
                        default: // square
                            ctx.fillRect(pos.x, pos.y, eyeSize, eyeSize);
                    }
                    
                    // Draw inner eye (for styles that have it)
                    if (style !== 'dot') {
                        const innerEyeSize = eyeSize * 0.4;
                        const innerEyePos = {
                            x: pos.x + (eyeSize - innerEyeSize)/2,
                            y: pos.y + (eyeSize - innerEyeSize)/2
                        };
                        
                        ctx.fillStyle = innerColor;
                        
                        switch(style) {
                            case 'circle':
                                ctx.beginPath();
                                ctx.arc(pos.x + eyeSize/2, pos.y + eyeSize/2, innerEyeSize/2, 0, Math.PI * 2);
                                ctx.fill();
                                break;
                            case 'rounded':
                                const innerCornerRadius = innerEyeSize * 0.2;
                                roundedRect(ctx, innerEyePos.x, innerEyePos.y, innerEyeSize, innerEyeSize, innerCornerRadius);
                                ctx.fill();
                                break;
                            case 'diamond':
                                diamondShape(ctx, innerEyePos.x, innerEyePos.y, innerEyeSize, innerEyeSize);
                                ctx.fill();
                                break;
                            case 'heart':
                                drawHeart(ctx, innerEyePos.x + innerEyeSize/2, innerEyePos.y + innerEyeSize/2, innerEyeSize/2);
                                ctx.fill();
                                break;
                            default: // square
                                ctx.fillRect(innerEyePos.x, innerEyePos.y, innerEyeSize, innerEyeSize);
                        }
                    }
                });
            }
            
            function roundedRect(ctx, x, y, width, height, radius) {
                ctx.beginPath();
                ctx.moveTo(x + radius, y);
                ctx.lineTo(x + width - radius, y);
                ctx.quadraticCurveTo(x + width, y, x + width, y + radius);
                ctx.lineTo(x + width, y + height - radius);
                ctx.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
                ctx.lineTo(x + radius, y + height);
                ctx.quadraticCurveTo(x, y + height, x, y + height - radius);
                ctx.lineTo(x, y + radius);
                ctx.quadraticCurveTo(x, y, x + radius, y);
                ctx.closePath();
            }
            
            function diamondShape(ctx, x, y, width, height) {
                ctx.beginPath();
                ctx.moveTo(x + width/2, y);
                ctx.lineTo(x + width, y + height/2);
                ctx.lineTo(x + width/2, y + height);
                ctx.lineTo(x, y + height/2);
                ctx.closePath();
            }
            
            function drawHeart(ctx, x, y, size) {
                ctx.beginPath();
                const topCurveHeight = size * 0.3;
                ctx.moveTo(x, y + topCurveHeight);
                // top left curve
                ctx.bezierCurveTo(
                    x, y, 
                    x - size, y, 
                    x - size, y + topCurveHeight
                );
                // bottom left curve
                ctx.bezierCurveTo(
                    x - size, y + (size + topCurveHeight) / 2, 
                    x, y + (size + topCurveHeight) / 2, 
                    x, y + size
                );
                // bottom right curve
                ctx.bezierCurveTo(
                    x, y + (size + topCurveHeight) / 2, 
                    x + size, y + (size + topCurveHeight) / 2, 
                    x + size, y + topCurveHeight
                );
                // top right curve
                ctx.bezierCurveTo(
                    x + size, y, 
                    x, y, 
                    x, y + topCurveHeight
                );
                ctx.closePath();
            }
            
            function addCenterLogo(canvas) {
                const ctx = canvas.getContext('2d');
                const size = canvas.width;
                const logoSize = size * 0.2;
                const logoX = (size - logoSize) / 2;
                const logoY = (size - logoSize) / 2;
                
                // Draw a simple logo placeholder (circle with initial)
                ctx.fillStyle = '#3b82f6';
                ctx.beginPath();
                ctx.arc(size/2, size/2, logoSize/2, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.fillStyle = 'white';
                ctx.font = `bold ${logoSize * 0.6}px Arial`;
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText('QR', size/2, size/2);
            }
            
            function addTextBelow(canvas, text) {
                const container = canvas.parentNode;
                const textElement = document.createElement('div');
                textElement.className = 'text-center mt-2 text-sm font-medium text-gray-700';
                textElement.textContent = text.length > 30 ? text.substring(0, 30) + '...' : text;
                container.appendChild(textElement);
            }
            
            function downloadQRCode(format) {
                const canvas = qrCodeContainer.querySelector('canvas');
                if (!canvas) return;
                
                let mimeType, extension;
                switch(format) {
                    case 'png':
                        mimeType = 'image/png';
                        extension = 'png';
                        break;
                    case 'jpg':
                        mimeType = 'image/jpeg';
                        extension = 'jpg';
                        break;
                    case 'svg':
                        // For SVG, we need to regenerate the QR code
                        const content = qrContent.value || 'https://example.com';
                        const color = qrColor.value.substring(1); // Remove #
                        const bgColor = qrBgColor.value.substring(1);
                        
                        QRCode.toString(content, {
                            type: 'svg',
                            color: {
                                dark: color,
                                light: bgColor
                            },
                            margin: 1
                        }, function(err, svg) {
                            if (err) throw err;
                            
                            // Create download link
                            const blob = new Blob([svg], {type: 'image/svg+xml'});
                            const url = URL.createObjectURL(blob);
                            const a = document.createElement('a');
                            a.href = url;
                            a.download = `qr-code.${extension}`;
                            document.body.appendChild(a);
                            a.click();
                            document.body.removeChild(a);
                            URL.revokeObjectURL(url);
                        });
                        return;
                    default:
                        return;
                }
                
                // For PNG and JPG
                const link = document.createElement('a');
                link.download = `qr-code.${extension}`;
                link.href = canvas.toDataURL(mimeType);
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            }
        });
    </script>
</body>
</html>
