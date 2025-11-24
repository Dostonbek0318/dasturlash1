<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>File Operations - C++ Fayl Operatsiyalari</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 30px;
            padding: 20px;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        .main-content {
            display: grid;
            grid-template-columns: 300px 1fr;
            gap: 20px;
            background: white;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }

        .sidebar {
            background: #2c3e50;
            color: white;
            padding: 20px;
            height: fit-content;
        }

        .sidebar h3 {
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #34495e;
        }

        .operations-list {
            list-style: none;
        }

        .operations-list li {
            padding: 12px 15px;
            margin-bottom: 5px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            border-left: 4px solid transparent;
        }

        .operations-list li:hover {
            background: #34495e;
            border-left-color: #3498db;
        }

        .operations-list li.active {
            background: #3498db;
            border-left-color: #2980b9;
        }

        .operation-content {
            padding: 30px;
        }

        .operation-card h2 {
            color: #2c3e50;
            margin-bottom: 25px;
            font-size: 1.8rem;
        }

        .operation-form {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 10px;
            margin-bottom: 25px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #2c3e50;
        }

        .input-group input, .input-group textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #e1e8ed;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        .input-group input:focus, .input-group textarea:focus {
            outline: none;
            border-color: #3498db;
        }

        button {
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
        }

        button:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.4);
        }

        button:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
            transform: none;
        }

        .result {
            background: #e8f4fc;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #3498db;
        }

        .result h3 {
            color: #2c3e50;
            margin-bottom: 10px;
        }

        .result pre {
            background: white;
            padding: 15px;
            border-radius: 5px;
            border: 1px solid #e1e8ed;
            white-space: pre-wrap;
            word-wrap: break-word;
            font-family: 'Courier New', monospace;
            max-height: 300px;
            overflow-y: auto;
        }

        .file-upload {
            background: #f8f9fa;
            border: 2px dashed #bdc3c7;
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            margin-bottom: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .file-upload:hover {
            border-color: #3498db;
            background: #e8f4fc;
        }

        .file-upload input {
            display: none;
        }

        .upload-text {
            color: #7f8c8d;
            font-size: 16px;
        }

        .upload-text i {
            font-size: 24px;
            margin-bottom: 10px;
            display: block;
        }

        .result-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .result-actions button {
            flex: 1;
            padding: 10px 15px;
            font-size: 14px;
        }

        .save-btn {
            background: linear-gradient(135deg, #27ae60, #2ecc71);
        }

        .save-btn:hover:not(:disabled) {
            box-shadow: 0 5px 15px rgba(39, 174, 96, 0.4);
        }

        .download-btn {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
        }

        .download-btn:hover:not(:disabled) {
            box-shadow: 0 5px 15px rgba(231, 76, 60, 0.4);
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .sidebar {
                order: 2;
            }
            
            .operation-content {
                order: 1;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .result-actions {
                flex-direction: column;
            }
        }

        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid #f3f3f3;
            border-top: 3px solid #3498db;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .file-content {
            margin-top: 10px;
            font-size: 14px;
            color: #666;
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="header">
            <h1>File Operations</h1>
            <p>C++ Fayl Operatsiyalari Veb Interfeysi</p>
        </header>

        <div class="main-content">
            <aside class="sidebar">
                <h3>Operatsiyalar</h3>
                <ul class="operations-list" id="operationsList">
                    <!-- Operations will be populated by JavaScript -->
                </ul>
            </aside>

            <main class="operation-content">
                <div class="operation-card">
                    <h2 id="operationTitle">Operatsiyani tanlang</h2>
                    
                    <div class="file-upload" onclick="document.getElementById('fileInput').click()">
                        <div class="upload-text">
                            <i>📁</i>
                            Fayl yuklash uchun bosing yoki faylni bu yerga tashlang
                        </div>
                        <input type="file" id="fileInput" onchange="handleFileUpload(this.files)">
                    </div>
                    
                    <form class="operation-form" id="operationForm">
                        <!-- Dynamic inputs will be inserted here -->
                        <div id="dynamicInputs"></div>
                        <button type="submit" id="submitBtn" disabled>
                            <span class="loading" id="loadingSpinner" style="display: none;"></span>
                            <span id="buttonText">Operatsiyani tanlang</span>
                        </button>
                    </form>

                    <div class="result" id="result" style="display: none;">
                        <h3>Natija:</h3>
                        <pre id="resultText"></pre>
                        <div class="file-content" id="fileContent" style="display: none;">
                            <strong>O'zgartirilgan fayl tarkibi:</strong>
                            <pre id="modifiedFileContent"></pre>
                        </div>
                        <div class="result-actions">
                            <button class="save-btn" onclick="saveResultToFile()">
                                Natijani faylga saqlash
                            </button>
                            <button class="download-btn" id="downloadFileBtn" onclick="downloadModifiedFile()" style="display: none;">
                                O'zgartirilgan faylni yuklab olish
                            </button>
                            <button onclick="copyResultToClipboard()">
                                Natijani nusxalash
                            </button>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <script>
        // Operations configuration
        const operations = [
            { id: 1, name: "Qatorlar soni", inputs: ['filename'] },
            { id: 2, name: "So'zlar soni", inputs: ['filename'] },
            { id: 3, name: "Faylni nusxalash", inputs: ['sourceFile', 'destFile'] },
            { id: 4, name: "So'zni almashtirish", inputs: ['filename', 'searchWord', 'replaceWord'] },
            { id: 5, name: "Qatorlarni tartiblash", inputs: ['filename'] },
            { id: 6, name: "Fayllarni birlashtirish", inputs: ['files', 'outputFile'] },
            { id: 7, name: "Faylni bo'lish", inputs: ['filename', 'partSize'] },
            { id: 8, name: "Qator qidirish", inputs: ['filename', 'searchLine'] },
            { id: 9, name: "Shifrlash", inputs: ['filename', 'key'] },
            { id: 10, name: "Deshifrlash", inputs: ['filename', 'key'] },
            { id: 11, name: "Raqamlar o'rtachasi", inputs: ['filename'] },
            { id: 12, name: "Eng uzun so'z", inputs: ['filename'] },
            { id: 13, name: "Belgilar soni", inputs: ['filename'] },
            { id: 14, name: "Teskari yozish", inputs: ['filename'] },
            { id: 15, name: "Raqamlarni ajratish", inputs: ['filename'] },
            { id: 16, name: "Unlilar soni", inputs: ['filename'] }
        ];

        let currentOperation = null;
        let uploadedFileName = '';
        let uploadedFileContent = '';
        let currentResult = '';
        let modifiedFileContent = '';

        // Initialize operations list
        function initializeOperations() {
            const operationsList = document.getElementById('operationsList');
            operations.forEach(op => {
                const li = document.createElement('li');
                li.textContent = `${op.id}. ${op.name}`;
                li.onclick = () => selectOperation(op);
                operationsList.appendChild(li);
            });
        }

        // Select operation
        function selectOperation(operation) {
            currentOperation = operation;
            
            // Update active state
            document.querySelectorAll('.operations-list li').forEach(li => {
                li.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Update title
            document.getElementById('operationTitle').textContent = 
                `${operation.id}. ${operation.name}`;
            
            // Update form inputs
            updateFormInputs(operation.inputs);
            
            // Update button
            const button = document.getElementById('submitBtn');
            const buttonText = document.getElementById('buttonText');
            button.disabled = false;
            buttonText.textContent = 'Bajarish';
        }

        // Update form inputs based on operation
        function updateFormInputs(inputs) {
            const container = document.getElementById('dynamicInputs');
            container.innerHTML = '';
            
            inputs.forEach(input => {
                const group = document.createElement('div');
                group.className = 'input-group';
                
                const label = document.createElement('label');
                label.textContent = getInputLabel(input);
                
                let inputElement;
                if (input === 'files') {
                    inputElement = document.createElement('textarea');
                    inputElement.placeholder = 'file1.txt, file2.txt, file3.txt';
                    inputElement.rows = 3;
                } else if (input === 'partSize') {
                    inputElement = document.createElement('input');
                    inputElement.type = 'number';
                    inputElement.placeholder = 'Qatorlar soni';
                } else {
                    inputElement = document.createElement('input');
                    inputElement.type = 'text';
                    inputElement.placeholder = `${getInputLabel(input)} kiriting...`;
                    
                    // Auto-fill filename if uploaded
                    if (input === 'filename' && uploadedFileName) {
                        inputElement.value = uploadedFileName;
                    }
                }
                
                inputElement.id = `input_${input}`;
                inputElement.oninput = () => validateForm();
                
                group.appendChild(label);
                group.appendChild(inputElement);
                container.appendChild(group);
            });
            
            validateForm();
        }

        // Get input label
        function getInputLabel(input) {
            const labels = {
                'filename': 'Fayl nomi',
                'sourceFile': 'Manba fayl',
                'destFile': 'Nishon fayl',
                'searchWord': 'Qidirilayotgan so\'z',
                'replaceWord': 'Almashtiriladigan so\'z',
                'files': 'Fayllar (vergul bilan ajratilgan)',
                'outputFile': 'Chiqish fayli',
                'partSize': 'Qism hajmi (qatorlar soni)',
                'searchLine': 'Qidirilayotgan qator',
                'key': 'Kalit'
            };
            return labels[input] || input;
        }

        // Handle file upload
        function handleFileUpload(files) {
            if (files.length > 0) {
                const file = files[0];
                uploadedFileName = file.name;
                
                const reader = new FileReader();
                reader.onload = function(e) {
                    uploadedFileContent = e.target.result;
                    const uploadText = document.querySelector('.upload-text');
                    uploadText.innerHTML = `<i>✅</i> ${uploadedFileName} - Yuklandi (${file.size} bayt)`;
                    
                    // Update filename inputs
                    const filenameInput = document.getElementById('input_filename');
                    if (filenameInput) {
                        filenameInput.value = uploadedFileName;
                        validateForm();
                    }
                };
                reader.readAsText(file);
            }
        }

        // Validate form
        function validateForm() {
            if (!currentOperation) return false;
            
            let isValid = true;
            currentOperation.inputs.forEach(input => {
                const inputElement = document.getElementById(`input_${input}`);
                if (inputElement && !inputElement.value.trim()) {
                    isValid = false;
                }
            });
            
            document.getElementById('submitBtn').disabled = !isValid;
            return isValid;
        }

        // Handle form submission
        document.getElementById('operationForm').onsubmit = async function(e) {
            e.preventDefault();
            
            if (!currentOperation || !validateForm()) return;
            
            const submitBtn = document.getElementById('submitBtn');
            const buttonText = document.getElementById('buttonText');
            const loadingSpinner = document.getElementById('loadingSpinner');
            
            // Show loading
            submitBtn.disabled = true;
            buttonText.textContent = 'Ishlayapti...';
            loadingSpinner.style.display = 'inline-block';
            
            // Collect form data
            const formData = {};
            currentOperation.inputs.forEach(input => {
                const inputElement = document.getElementById(`input_${input}`);
                formData[input] = inputElement.value.trim();
            });
            
            try {
                // Simulate API call - in real implementation, this would call your backend
                const result = await simulateBackendCall(currentOperation.id, formData, uploadedFileContent);
                
                // Show result
                currentResult = result.message;
                modifiedFileContent = result.modifiedContent || '';
                
                document.getElementById('resultText').textContent = currentResult;
                document.getElementById('result').style.display = 'block';
                
                // Show modified file content and download button if file was modified
                if (result.modifiedContent) {
                    document.getElementById('modifiedFileContent').textContent = result.modifiedContent;
                    document.getElementById('fileContent').style.display = 'block';
                    document.getElementById('downloadFileBtn').style.display = 'block';
                } else {
                    document.getElementById('fileContent').style.display = 'none';
                    document.getElementById('downloadFileBtn').style.display = 'none';
                }
                
            } catch (error) {
                currentResult = 'Xatolik: ' + error.message;
                document.getElementById('resultText').textContent = currentResult;
                document.getElementById('result').style.display = 'block';
                document.getElementById('fileContent').style.display = 'none';
                document.getElementById('downloadFileBtn').style.display = 'none';
            } finally {
                // Reset button
                submitBtn.disabled = false;
                buttonText.textContent = 'Bajarish';
                loadingSpinner.style.display = 'none';
            }
        };

        // Save result to file
        function saveResultToFile() {
            if (!currentResult) return;
            
            // Create a blob with the result text
            const blob = new Blob([currentResult], { type: 'text/plain' });
            
            // Create a temporary URL for the blob
            const url = URL.createObjectURL(blob);
            
            // Create a temporary link element
            const a = document.createElement('a');
            a.href = url;
            
            // Generate filename based on operation
            const operationName = currentOperation ? currentOperation.name.replace(/\s+/g, '_') : 'natija';
            const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
            a.download = `${operationName}_${timestamp}.txt`;
            
            // Trigger download
            document.body.appendChild(a);
            a.click();
            
            // Clean up
            setTimeout(() => {
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
            }, 100);
        }

        // Download modified file
        function downloadModifiedFile() {
            if (!modifiedFileContent) return;
            
            // Create a blob with the modified file content
            const blob = new Blob([modifiedFileContent], { type: 'text/plain' });
            
            // Create a temporary URL for the blob
            const url = URL.createObjectURL(blob);
            
            // Create a temporary link element
            const a = document.createElement('a');
            a.href = url;
            
            // Generate filename based on original file name and operation
            const originalName = uploadedFileName.split('.')[0];
            const extension = uploadedFileName.split('.')[1] || 'txt';
            const operationName = currentOperation ? currentOperation.name.replace(/\s+/g, '_') : 'modified';
            a.download = `${originalName}_${operationName}.${extension}`;
            
            // Trigger download
            document.body.appendChild(a);
            a.click();
            
            // Clean up
            setTimeout(() => {
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
            }, 100);
        }

        // Copy result to clipboard
        function copyResultToClipboard() {
            if (!currentResult) return;
            
            navigator.clipboard.writeText(currentResult)
                .then(() => {
                    // Show temporary success message
                    const saveBtn = document.getElementById('saveResultBtn');
                    const originalText = saveBtn.textContent;
                    saveBtn.textContent = 'Nusxalandi!';
                    saveBtn.style.background = 'linear-gradient(135deg, #27ae60, #2ecc71)';
                    
                    setTimeout(() => {
                        saveBtn.textContent = originalText;
                        saveBtn.style.background = 'linear-gradient(135deg, #3498db, #2980b9)';
                    }, 2000);
                })
                .catch(err => {
                    console.error('Nusxalashda xatolik: ', err);
                    alert('Nusxalashda xatolik yuz berdi');
                });
        }

        // Simulate backend call - Replace this with actual API calls
        async function simulateBackendCall(operationId, formData, fileContent) {
            // Simulate network delay
            await new Promise(resolve => setTimeout(resolve, 2000));
            
            // Simulate file processing based on operation
            let modifiedContent = '';
            let message = '';
            
            switch(operationId) {
                case 1: // Qatorlar soni
                    const lines = fileContent.split('\n').length;
                    message = `Faylda ${lines} ta qator mavjud`;
                    break;
                    
                case 2: // So'zlar soni
                    const words = fileContent.split(/\s+/).filter(word => word.length > 0).length;
                    message = `Faylda ${words} ta so'z mavjud`;
                    break;
                    
                case 4: // So'zni almashtirish
                    modifiedContent = fileContent.replace(new RegExp(formData.searchWord, 'g'), formData.replaceWord);
                    const replacements = (fileContent.match(new RegExp(formData.searchWord, 'g')) || []).length;
                    message = `${replacements} ta so'z almashtirildi`;
                    break;
                    
                case 5: // Qatorlarni tartiblash
                    const sortedLines = fileContent.split('\n').sort();
                    modifiedContent = sortedLines.join('\n');
                    message = 'Qatorlar alifbo tartibida tartiblandi';
                    break;
                    
                case 8: // Qator qidirish
                    const foundLines = fileContent.split('\n').filter(line => 
                        line.includes(formData.searchLine)
                    );
                    message = foundLines.length > 0 
                        ? `Topilgan qatorlar:\n${foundLines.join('\n')}`
                        : 'Qator topilmadi';
                    break;
                    
                case 9: // Shifrlash
                    modifiedContent = btoa(fileContent); // Base64 encoding as simple "encryption"
                    message = 'Fayl muvaffaqiyatli shifrlandi';
                    break;
                    
                case 10: // Deshifrlash
                    try {
                        modifiedContent = atob(fileContent);
                        message = 'Fayl muvaffaqiyatli deshifrlandi';
                    } catch (e) {
                        message = 'Faylni deshifrlashda xatolik';
                    }
                    break;
                    
                case 14: // Teskari yozish
                    modifiedContent = fileContent.split('').reverse().join('');
                    message = 'Matn teskari tartibda yozildi';
                    break;
                    
                case 15: // Raqamlarni ajratish
                    const numbers = fileContent.match(/\d+/g) || [];
                    modifiedContent = numbers.join('\n');
                    message = `Fayldan ${numbers.length} ta raqam ajratildi`;
                    break;
                    
                case 16: // Unlilar soni
                    const vowels = fileContent.match(/[aeiouAEIOUаеёиоуыэюяАЕЁИОУЫЭЮЯ]/g) || [];
                    message = `Faylda ${vowels.length} ta unli mavjud`;
                    break;
                    
                default:
                    message = 'Operatsiya muvaffaqiyatli bajarildi';
                    modifiedContent = fileContent; // Return original content for other operations
            }
            
            return {
                message: message,
                modifiedContent: modifiedContent
            };
        }

        // Drag and drop functionality
        document.addEventListener('DOMContentLoaded', function() {
            const uploadArea = document.querySelector('.file-upload');
            
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                uploadArea.addEventListener(eventName, preventDefaults, false);
            });
            
            function preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }
            
            ['dragenter', 'dragover'].forEach(eventName => {
                uploadArea.addEventListener(eventName, highlight, false);
            });
            
            ['dragleave', 'drop'].forEach(eventName => {
                uploadArea.addEventListener(eventName, unhighlight, false);
            });
            
            function highlight() {
                uploadArea.style.borderColor = '#3498db';
                uploadArea.style.background = '#e8f4fc';
            }
            
            function unhighlight() {
                uploadArea.style.borderColor = '#bdc3c7';
                uploadArea.style.background = '#f8f9fa';
            }
            
            uploadArea.addEventListener('drop', handleDrop, false);
            
            function handleDrop(e) {
                const dt = e.dataTransfer;
                const files = dt.files;
                handleFileUpload(files);
            }
            
            // Initialize operations
            initializeOperations();
        });
    </script>
</body>
</html>
