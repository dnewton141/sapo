Ink.createModule('Ink.Login.Login', '1', ['Ink.Dom.Event_1', 'Ink.Dom.Css_1', 'Ink.Net.Ajax_1', 'Ink.Dom.Element'], function(InkEvent, InkCss, InkAjax, InkElement){
    var Login = {
        addParameter: function(url, key, val) {
            var newParam = escape(key) + '=' + escape(val);
            var newUrl = url;

            if (url.indexOf('?') == -1) {
                newUrl += '?' + newParam;
            } else {
                newUrl += '&' + newParam;
            }
            return newUrl;
        },
        checkPersistent: function(e) {
           var refValue = Ink.i('check-ligado').checked ? "1" : "0";

           var elm = Ink.i('checkPersistentFacebook');
           elm.value = refValue;
           elm = Ink.i('checkPersistentLiveId');
           elm.value = refValue;
           elm = Ink.i('checkPersistentOpenId');
           elm.value = refValue;
           elm = Ink.i('checkPersistentCc');
           elm.value = refValue;
        },
        renewCaptcha: function(type, id, callback) {
            new InkAjax('/Captcha.do?action=renew&type=' + type + '&id=' + id, {
                    method: 'GET',
                    onComplete: function (a) {
                        if (a.responseJSON.status == 0) {
                            return callback(a.responseJSON);
                        } else {
                            return callback('');
                        }
                    },
                    onFailure: function (a) {
                        return callback('');
                    }
                });
        },
        renewCaptchaImage: function (e) {
            InkEvent.stop(e);
            var id = escape(Ink.i('widget_captcha_id').value);
            Ink.Login.Login.renewCaptcha('image', id, function(captcha) {
                if (captcha) {
                    Ink.i('widget_captcha_sound').innerHTML = '';
                    Ink.i('widget_captcha_img').src = '/Captcha.do?action=show&id=' + captcha.id;
                    Ink.i('widget_captcha_id').value = captcha.id;
                    Ink.i('widget_captcha_img').style.visibility = 'visible';
                    Ink.i('widget_captcha_code').value = '';
                    Ink.i('widget_captcha_code').focus();
                } else {
                    // notify user
                }
            });
        },
        playCaptcha: function (e) {
            InkEvent.stop(e);
            Ink.i('widget_captcha_code').value = '';
            Ink.i('widget_captcha_img').style.visibility = 'hidden';
            Ink.i('widget_captcha_sound').innerHTML = '';
            if (Ink.Login.Login.soundCaptchaId) {
                Ink.i('widget_captcha_id').value = Ink.Login.Login.soundCaptchaId;
                Ink.i('widget_captcha_sound').innerHTML = '<embed src="/Captcha.do?action=play&id=' + Ink.Login.Login.soundCaptchaId + '" autostart="true" style="border: 1px; position: absolute; top: -9999px; height: -9999px" />';
            } else {
                var id = escape(Ink.i('widget_captcha_id').value);
                Ink.Login.Login.renewCaptcha('sound', id, function(captcha) {
                    if (captcha) {
                        Ink.Login.Login.soundCaptchaId = captcha.id;
                        Ink.i('widget_captcha_id').value = captcha.id;
                        Ink.i('widget_captcha_sound').innerHTML = '<embed src="/Captcha.do?action=play&id=' + captcha.id + '" autostart="true" style="border: 1px; position: absolute; top: -9999px; height: -9999px" />';
                    }
                });
            }
        },
        soundCaptchaId: null
    };
    return Login;
});

Ink.requireModules(['Ink.Dom.Loaded_1', 'Ink.Dom.Event_1', 'Ink.Dom.Css_1'], function(Loaded, InkEvent, InkCss){
    Loaded.run (function(){
        // captcha renew link and sound player
        if (Ink.i('widget_captcha_id')) {
            var elem = Ink.i('widget_captcha_renew_a');
            InkCss.removeClassName(elem, 'displayNone');
            InkEvent.observe(Ink.i('widget_captcha_renew'), 'click', Ink.bindEvent(Ink.Login.Login.renewCaptchaImage, this));
            InkEvent.observe(Ink.i('widget_captcha_play'), 'click', Ink.bindEvent(Ink.Login.Login.playCaptcha, this));
        }
        if (Ink.i('check-ligado')) {
            InkEvent.observe(Ink.i('check-ligado'), 'click', Ink.bindEvent(Ink.Login.Login.checkPersistent, this));
        }
    });
});

//Create Element.remove() function if not exist
if (!('remove' in Element.prototype)) {
    Element.prototype.remove = function() {
        if (this.parentNode) {
            this.parentNode.removeChild(this);
        }
    };
}

function choose2FA(){
    document.getElementById('sms-and-totp').style.display = 'block';
    if(document.getElementById('send_sms')){
        document.getElementById('send_sms').remove();
    }

    if(document.getElementById('select_method')){
        document.getElementById('select_method').remove();
    }

    if(document.getElementById('before_click')){
        document.getElementById('before_click').remove();
    }

    document.getElementById('after_click').style.display = 'block';
    document.getElementById("text-otp").focus();
}
