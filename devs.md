<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="robots" content="noindex, nofollow">
    <title>Password Protected Page</title>
    <style>
        html, body {
            margin: 0;
            width: 100%;
            height: 100%;
            font-family: Arial, Helvetica, sans-serif;
        }
        #dialogText {
            padding: 10px 30px;
            color: white;
            background-color: #333333;
        }
        
        #dialogWrap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: table;
            background-color: #EEEEEE;
        }
        
        #dialogWrapCell {
            display: table-cell;
            text-align: center;
            vertical-align: middle;
        }
        
        #mainDialog {
            width: 500px;
            border: solid #AAAAAA 1px;
            border-radius: 10px;
            box-shadow: 3px 3px 5px 3px #AAAAAA;
            margin-left: auto;
            margin-right: auto;
            background-color: #FFFFFF;
            overflow: hidden;
            text-align: left;
        }
        #passArea {
            padding: 20px 30px;
            background-color: white;
        }
        #passArea > * {
            margin: 5px auto;
        }
        #pass {
            width: 100%;
            height: 40px;
            font-size: 30px;
        }
        
        #messageWrapper {
            float: left;
            vertical-align: middle;
            line-height: 30px;
        }
        
        .notifyText {
            display: none;
        }
        
        #invalidPass {
            color: red;
        }
        
        #success {
            color: green;
        }
        
        #submitPass {
            font-size: 20px;
            border-radius: 5px;
            background-color: #E7E7E7;
            border: solid gray 1px;
            float: right;
        }
        #contentFrame {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
    </style>
  </head>
  <body>
    <iframe id="contentFrame" frameBorder="0"></iframe>
    <div id="dialogWrap">
        <div id="dialogWrapCell">
            <div id="mainDialog">
                <div id="dialogText">This page is password protected.</div>
                <div id="passArea">
                    <p id="passwordPrompt">Password</p>
                    <input id="pass" type="password" name="pass">
                    <div>
                        <span id="messageWrapper">
                            <span id="invalidPass" class="notifyText">Sorry, please try again.</span>
                            <span id="success" class="notifyText">Success!</span>
                            &nbsp;
                        </span>
                        <button id="submitPass" type="button">Submit</button>
                        <div style="clear: both;"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.2/rollups/aes.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.2/rollups/pbkdf2.js"></script>
    <script>
        /*! srcdoc-polyfill - v0.1.1 - 2013-03-01
        * http://github.com/jugglinmike/srcdoc-polyfill/
        * Copyright (c) 2013 Mike Pennisi; Licensed MIT */
        (function( window, document, undefined ) {
	
	        var idx, iframes;
	        var _srcDoc = window.srcDoc;
	        var isCompliant = !!("srcdoc" in document.createElement("iframe"));
	        var implementations = {
		        compliant: function( iframe, content ) {

			        if (content) {
				        iframe.setAttribute("srcdoc", content);
			        }
		        },
		        legacy: function( iframe, content ) {

			        var jsUrl;

			        if (!iframe || !iframe.getAttribute) {
				        return;
			        }

			        if (!content) {
				        content = iframe.getAttribute("srcdoc");
			        } else {
				        iframe.setAttribute("srcdoc", content);
			        }

			        if (content) {
				        // The value returned by a script-targeted URL will be used as
				        // the iFrame's content. Create such a URL which returns the
				        // iFrame element's `srcdoc` attribute.
				        jsUrl = "javascript: window.frameElement.getAttribute('srcdoc');";

				        iframe.setAttribute("src", jsUrl);

				        // Explicitly set the iFrame's window.location for
				        // compatability with IE9, which does not react to changes in
				        // the `src` attribute when it is a `javascript:` URL, for
				        // some reason
				        if (iframe.contentWindow) {
					        iframe.contentWindow.location = jsUrl;
				        }
			        }
		        }
	        };
	        var srcDoc = window.srcDoc = {
		        // Assume the best
		        set: implementations.compliant,
		        noConflict: function() {
			        window.srcDoc = _srcDoc;
			        return srcDoc;
		        }
	        };

	        // If the browser supports srcdoc, no shimming is necessary
	        if (isCompliant) {
		        return;
	        }

	        srcDoc.set = implementations.legacy;

	        // Automatically shim any iframes already present in the document
	        iframes = document.getElementsByTagName("iframe");
	        idx = iframes.length;

	        while (idx--) {
		        srcDoc.set( iframes[idx] );
	        }

        }( this, this.document ));
    </script>
    <script>
        var pl = {"salt":"ae7g8gbRh+RGDfjHVEN671axkY5sp5TG4ENR0947HrA=","iv":"VljhVz/WEhFr8bL3IJBIZQ==","data":"V8NEmS+AjfdblD53LzHd2c2GApI0r+FmyXR9947Fps1Zn7MPW7WYle13flcCcEvCxzWkg17I0LPoy1y6Y5SpC8fD52dxgh+X37LuocHOp42pgvFbai+qCpXbsiUaROsMz+6NmMvnwaFiK8Nv7avc4YiTwXOCbkNFd/kwfTwTzbpG67YrEKU/+T+llUYThleuvKCyxy9OE/3Z/auWax7T34StuqrpMp9Ln/QILin0npJTaFvdbF3recrBIGiSRaybM5UhZiDhazYyjA4o4AxwCB6uf+JqopPXL0vocqotCUgWmRZAYp0bbVESIiL50jJaN6Xjq0BShrtiUsoyUVB5yh6hr/18tl5ebJ87jCKUfjNQaHWWCurPI93eQk4bSI3rismZ1LjTDM1gf9piq8n9DUWDCytlFytuk55/gz6DGSbZ99+Y9zmjocZzkGNbubSNYg28saF5+4+lDV/OMxGMtFVYN324FejPj5tFu1uNtToSMzOvDSnioiPY+tP5MPyzBtQDPAtA+axfZsfztH0fULr4XeGFFnl6KbYk8eWJcPmJ6Zkn4XarQ0X4WeZVkrTS3BrtnGsNFfs4nfXJqo19AMGhdsxlSu4+mhcbx+R/p6EhUEfoOWL94GnVDOgYDDQMvlNJgPRYq2scpa+4QiOoGFHHMN6jbH3n3lIYB4kOn2jmnGO64yJl/wry8cuk7W0463nlv4zETpd5ey/s+ynnUHp2Apbr6QiwhIQtfEBZgMP1VlvVWhxoGvqMdEyO5WtUfVmfCwP0XWaYGqExswtPJJzlMF/MbTyQ0Q86Ez5haEA1/Is7FIQ/8Lig5lnsbX3uoXERdLRhSViucaJ3EK7lSAlOTnW7ZPfAyxFMD3ohwMHyfIBAztyqa65wi0faZW91y0jXocp1afwB/euZXkVEkbFstMK8bF27JcYq7/TLm14SJokdZEu7wVAb3NGQX8mUD+kwkcHFgEYX8kxB45xDvg=="};
        
        var submitPass = document.getElementById('submitPass');
        var passEl = document.getElementById('pass');
        var invalidPassEl = document.getElementById('invalidPass');
        var successEl = document.getElementById('success');
        var contentFrame = document.getElementById('contentFrame');
        
        if (pl === "") {
            submitPass.disabled = true;
            passEl.disabled = true;
            alert("This page is meant to be used with the encryption tool. It doesn't work standalone.");
        }
        
        function doSubmit(evt) {
            try {
                var decrypted = decryptFile(CryptoJS.enc.Base64.parse(pl.data), passEl.value, CryptoJS.enc.Base64.parse(pl.salt), CryptoJS.enc.Base64.parse(pl.iv));
                if (decrypted === "") throw "No data returned";
                
                // Set default iframe link targets to _top so all links break out of the iframe
                decrypted = decrypted.replace("<head>", "<head><base href=\".\" target=\"_top\">");
                
                srcDoc.set(contentFrame, decrypted);
                
                successEl.style.display = "inline";
                passEl.disabled = true;
                submitPass.disabled = true;
                setTimeout(function() {
                    dialogWrap.style.display = "none";
                }, 1000);
            } catch (e) {
                invalidPassEl.style.display = "inline";
                passEl.value = "";
            }
        }
        
        submitPass.onclick = doSubmit;
        passEl.onkeypress = function(e){
            if (!e) e = window.event;
            var keyCode = e.keyCode || e.which;
            invalidPassEl.style.display = "none";
            if (keyCode == '13'){
              // Enter pressed
              doSubmit();
              return false;
            }
        }
        
        function decryptFile(contents, password, salt, iv) {
            var _cp = CryptoJS.lib.CipherParams.create({
                ciphertext: contents
            });
            var key = CryptoJS.PBKDF2(password, salt, { keySize: 256/32, iterations: 100 });
            var decrypted = CryptoJS.AES.decrypt(_cp, key, {iv: iv});
            
            return decrypted.toString(CryptoJS.enc.Utf8);
        }
    </script>
  </body>
</html>
