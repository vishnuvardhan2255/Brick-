<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Code Compiler</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
        }
        textarea {
            width: 100%;
            height: 150px;
            margin-bottom: 10px;
            font-family: monospace;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
        button {
            display: block;
            margin: 10px 0;
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            background-color: #218838;
        }
        iframe {
            width: 100%;
            height: 300px;
            border: 1px solid #ccc;
            background: white;
        }
        #pythonOutput {
            width: 100%;
            height: 150px;
            background: white;
            border: 1px solid #ccc;
            padding: 10px;
            border-radius: 5px;
            overflow-y: auto;
        }
    </style>
</head>
<body>
    <h2>Online Code Compiler</h2>
    <label>HTML</label>
    <textarea id="htmlCode" placeholder="Write your HTML here..."></textarea>
    <label>CSS</label>
    <textarea id="cssCode" placeholder="Write your CSS here..."></textarea>
    <label>JavaScript</label>
    <textarea id="jsCode" placeholder="Write your JavaScript here..."></textarea>
    <button onclick="compileCode()">Run Code</button>
    <h3>Output:</h3>
    <iframe id="output"></iframe>

    <label>Python</label>
    <textarea id="pythonCode" placeholder="Write your Python here..."></textarea>
    <button onclick="runPython()">Run Python</button>
    <h3>Python Output:</h3>
    <pre id="pythonOutput"></pre>

    <script>
        function compileCode() {
            let html = document.getElementById("htmlCode").value;
            let css = "<style>" + document.getElementById("cssCode").value + "</style>";
            let js = "<script>" + document.getElementById("jsCode").value + "</" + "script>";
            let output = document.getElementById("output").contentWindow.document;
            output.open();
            output.write(html + css + js);
            output.close();
        }

        function runPython() {
            let code = document.getElementById("pythonCode").value;
            fetch("https://emkc.org/api/v2/piston/execute", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    language: "python",
                    version: "3.10.0",
                    files: [{ content: code }]
                })
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById("pythonOutput").innerText = data.run.output;
            })
            .catch(error => {
                document.getElementById("pythonOutput").innerText = "Error executing Python code";
            });
        }
    </script>
</body>
</html>
