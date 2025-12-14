FROM node:16-alpine

WORKDIR /app

# Install MeshCommander
RUN npm install meshcommander

# Change computer list path to persistent volume
RUN sed -i "s|readFile('computerlist.config'|readFile('/app/data/computerlist.config'|g" \
    /app/node_modules/meshcommander/webserver.js

# Keep passwords in response (original strips them)
RUN node -e "const fs=require('fs'); \
    let c=fs.readFileSync('/app/node_modules/meshcommander/webserver.js','utf8'); \
    c=c.replace(/for \\(var i in list\\) \\{ delete list\\[i\\]\\.pass; \\} \\/\\/ Remove all passwords\\./g, \
    '// Passwords kept in response'); \
    fs.writeFileSync('/app/node_modules/meshcommander/webserver.js',c);"

# Add POST endpoint to save computer list
RUN node -e "const fs=require('fs'); \
    let c=fs.readFileSync('/app/node_modules/meshcommander/webserver.js','utf8'); \
    if(c.indexOf('action == \"savecomputerlist\"')===-1){ \
        const p=c.indexOf('    return obj;'); \
        if(p!==-1){ \
            const n='    obj.app.post(\"/webrelay.ashx\", function (req, res) {\n\
        if (req.query.action == \"savecomputerlist\") {\n\
            var body = \"\";\n\
            req.on(\"data\", function (chunk) { body += chunk.toString(); });\n\
            req.on(\"end\", function () {\n\
                try {\n\
                    obj.fs.writeFile(\"/app/data/computerlist.config\", body, \"utf8\", function (err) {\n\
                        if (err) { res.status(500).send(\"Error: \" + err.message); }\n\
                        else { res.set({ \"Content-Type\": \"application/json\" }); res.send(JSON.stringify({ success: true })); }\n\
                    });\n\
                } catch (e) { res.status(500).send(\"Error: \" + e.message); }\n\
            });\n\
        } else { res.status(404).send(\"Not found\"); }\n\
    });\n'; \
            c=c.slice(0,p)+n+c.slice(p); \
            fs.writeFileSync('/app/node_modules/meshcommander/webserver.js',c); \
        } \
    }"

# Auto-save to server when computers are saved
RUN sed -i "s|localStorage.setItem('computers', JSON.stringify(computerlist));|\
localStorage.setItem('computers', JSON.stringify(computerlist)); \
try { var xhr = new XMLHttpRequest(); \
xhr.open('POST', '/webrelay.ashx?action=savecomputerlist', true); \
xhr.setRequestHeader('Content-Type', 'application/json'); \
var data = JSON.stringify({ 'webappversion':typeof version != 'undefined' ? version : '0.9.5', 'computers':computerlist }); \
xhr.send(data); } catch (ex) {}|" /app/node_modules/meshcommander/public/default.htm

# Auto-load from server on startup (ensures computers have proper identification)
RUN sed -i "/var ctext = null;/a\
            try { var xhr = new XMLHttpRequest(); xhr.open('GET', '/webrelay.ashx?action=getcomputerlist', false); xhr.send(); \
            if (xhr.status == 200 && xhr.responseText) { try { var serverData = JSON.parse(xhr.responseText); \
            if (serverData && serverData.computers) { computerlist = serverData.computers; \
            for (var i = 0; i < computerlist.length; i++) { if (!computerlist[i]['h']) computerlist[i]['h'] = Math.random(); \
            if (!computerlist[i]['tls']) computerlist[i]['tls'] = 0; } ctext = JSON.stringify(computerlist); } } catch (e) {} } } catch (ex) {}" \
    /app/node_modules/meshcommander/public/default.htm

# Create data directory
RUN mkdir -p /app/data

EXPOSE 3000

CMD ["node", "/app/node_modules/meshcommander/meshcommander.js", "--any"]
