# codegather
代码记录

#2016年9月21日16:56:27
#登陆注册
var sign = {};

// 获取dom
sign.getDom = function () {
	var signWrap = document.getElementById('sign-wrap') || null,
	       loginTit = signWrap.getElementsByClassName('sign-login-tit')[0] || null,
		   regitarTit = signWrap.getElementsByClassName('sign-register-tit')[0] || null,
		   login = signWrap.getElementsByClassName('sign-login')[0] || null,
		   regitar = signWrap.getElementsByClassName('sign-regitar')[0] || null,
		   loginEmail = signWrap.getElementsByClassName('sign-login-email')[0] || null,
		   loginPass = signWrap.getElementsByClassName('sign-login-password')[0] || null,
		   loginBtn = signWrap.getElementsByClassName('sign-login-btn')[0] || null,
		   regitarEmail = signWrap.getElementsByClassName('sign-regitar-email')[0] || null,
		   regitarPass = signWrap.getElementsByClassName('sign-regitar-password')[0] || null,
		   regitarBtn = signWrap.getElementsByClassName('sign-login-btn')[0] || null,
		   regitarAffirmPassword = signWrap.getElementsByClassName('sign-regitar-affirm-password')[0] || null,
		   regitarTel = signWrap.getElementsByClassName('sign-regitar-tel')[0] || null,
		   regitarNote = signWrap.getElementsByClassName('sign-regitar-note')[0] || null,
		   regitarSetVerificat = signWrap.getElementsByClassName('sign-regitar-set-verificat')[0] || null,
		   regitarVerificatCode = signWrap.getElementsByClassName('sign-regitar-verificat-code')[0] || null,
		   regitarImgVerificat = signWrap.getElementsByClassName('sign-regitar-img-verificat')[0] || null,
		   signAgreement = signWrap.getElementsByClassName('sign-agreement')[0] || null,
		   registerBtn = signWrap.getElementsByClassName('sign-register-btn')[0] || null;
		   registerErrorTint = signWrap.getElementsByClassName('sign-regitar-error-hint')[0] || null;
		   loginErrorTint = signWrap.getElementsByClassName('sign-login-error-hint')[0] || null;
		   
	sign.dom = {
		"signWrap": signWrap,
		"loginTit": loginTit,
		"regitarTit": regitarTit,
		"login": login,
		"regitar": regitar,
		"loginEmail": loginEmail,
		"loginPass": loginPass,
		"loginBtn": loginBtn,
		"regitarEmail": regitarEmail,
		"regitarPass": regitarPass,
		"regitarBtn": regitarBtn,
		"regitarAffirmPassword": regitarAffirmPassword,
		"regitarTel": regitarTel,
		"regitarNote": regitarNote,
		"regitarSetVerificat": regitarSetVerificat,
		"regitarVerificatCode": regitarVerificatCode,
		"regitarImgVerificat": regitarImgVerificat,
		"signAgreement": signAgreement,
		"registerBtn": registerBtn,
		"registerErrorTint": registerErrorTint,
		"loginErrorTint": loginErrorTint
	};
};

// tab选项栏
sign.tab = function () {
	sign.dom.loginTit.onclick = function () {
		sign.dom.regitarTit.className = "login-tit";
		this.className = "login-tit on";
		
		sign.dom.login.style.display = "block";
		sign.dom.regitar.style.display = "none";
	}
	
	sign.dom.regitarTit.onclick = function () {
		sign.dom.loginTit.className = "regitar-tit";
		this.className = "regitar-tit on";
		
		sign.dom.regitar.style.display = "block";
		sign.dom.login.style.display = "none";
	}
}

// 显示
sign.show = function (state) {
	if (state === 0) {  // 登陆
		sign.dom.regitarTit.className = "regitar-tit";
		sign.dom.loginTit.className = "login-tit on";
		
		sign.dom.login.style.display = "block";
		sign.dom.regitar.style.display = "none";
	} else {  // 注册
		sign.dom.loginTit.className = "login-tit";
		sign.dom.regitarTit.className = "regitar-tit on";
		
		sign.dom.regitar.style.display = "block";
		sign.dom.login.style.display = "none";
	}
	sign.dom.signWrap.style.display = "block";
}

// 隐藏
sign.hide = function () {
	sign.dom.signWrap.style.display = "none";
}

// 登录
sign.login = function () {
	sign.dom.loginEmail.onblur = function () {
		if (!sign.dom.loginEmail || !sign.isEmail(sign.dom.loginEmail.value)) {
			sign.dom.loginErrorTint.innerHTML = "请输入正确的邮箱！";
			sign.dom.loginEmail.className = "sign-login-email error-active";
			return;
		} else {
			sign.dom.loginErrorTint.innerHTML = "";
			sign.dom.loginEmail.className = "sign-login-email success-active";
		}
	}
	
	sign.dom.loginPass.onblur = function () {
		if (!sign.dom.loginPass || !sign.dom.loginPass.value) {
			sign.dom.loginErrorTint.innerHTML = "请输入密码！";
			sign.dom.loginPass.className = "sign-login-email error-active";
			return;
		} else {
			sign.dom.loginErrorTint.innerHTML = "";
			sign.dom.loginPass.className = "sign-login-email success-active";
		}
	}
	
	sign.dom.loginBtn.onclick = function () {
		sign.ajax("/user/login/index.html?random="+Math.round(Math.random()*100)+"&loginName="+sign.dom.loginEmail.value+"&password="+sign.dom.loginPass.value+"&longLogin=0",function (call) {
			if(call!=null){
				switch (call.resultCode) {
					case -1 :
						sign.dom.loginErrorTint.innerHTML = "用户名或密码错误";
						break;
					case -2 :
						sign.dom.loginErrorTint.innerHTML = "此ip登录频繁，请2小时后再试";
						break;
					case -3 :
						sign.dom.loginErrorTint.innerHTML = "";
						if(call.errorNum == 0){
							sign.dom.loginErrorTint.innerHTML="此ip登录频繁，请2小时后再试";
						}else{
							sign.dom.loginErrorTint.innerHTML="用户名或密码错误，您还有"+call.errorNum+"次机会";
						}
						break;
					case -4 :
						sign.dom.loginErrorTint.innerHTML = "您的浏览器还未开启COOKIE,请设置启用COOKIE功能";
						break;
					case 1 :
						console.log(location.search.match("forwardUrl"));
						console.log(location.search.split('=')[1] !== "");
						if (location.search.match("forwardUrl") && location.search.split('=')[1] !== "") {
							window.location.href = "http://"+window.location.host+"/" + location.search.split('=')[1];
						} else if (location.search.match("forwardUrl") && location.search.split('=')[1].trim() == "") {
							window.location.href = "http://"+window.location.host+"/";
						} else {
							window.location.href = "http://"+window.location.host+"/";
						}
						break;
					case 2 : 
						sign.dom.loginErrorTint.innerHTML = "账户出现安全隐患被冻结，请尽快联系客服。";
						break;
					case -404 :
						sign.dom.loginErrorTint.innerHTML = "系统升级中，暂停登录";
						break;
				}
			}
		});
	}
}

// 注册
sign.regitar = function () {
	sign.dom.regitarEmail.onblur = function () {
		if (!sign.dom.regitarEmail || !sign.isEmail(sign.dom.regitarEmail.value)) {
			sign.dom.registerErrorTint.innerHTML = "请输入正确的邮箱！";
			sign.dom.regitarEmail.className = "sign-regitar-email error-active";
			return;
		} else {
			sign.dom.registerErrorTint.innerHTML = "";
			sign.dom.regitarEmail.className = "sign-regitar-email success-active";
		}
	}

	sign.dom.regitarPass.onblur = function () {
		if (!sign.dom.regitarPass || !sign.dom.regitarPass.value) {
			sign.dom.registerErrorTint.innerHTML = "请输入密码！";
			sign.dom.regitarPass.className = "sign-regitar-password error-active";
			return;
		} else {
			sign.dom.registerErrorTint.innerHTML = "";
			sign.dom.regitarPass.className = "sign-regitar-password success-active";
		}
	}

	sign.dom.regitarAffirmPassword.onblur = function () {
		if (!sign.dom.regitarAffirmPassword || !sign.dom.regitarAffirmPassword.value) {
			sign.dom.registerErrorTint.innerHTML = "请输入确认密码！";
			sign.dom.regitarAffirmPassword.className = "sign-regitar-affirm-password error-active";
			return;
		} else if (sign.dom.regitarAffirmPassword.value !== sign.dom.regitarPass.value) {
			sign.dom.registerErrorTint.innerHTML = "两次输入的密码不一致！";
			sign.dom.regitarAffirmPassword.className = "sign-regitar-affirm-password error-active";
			return;
		} else {
			sign.dom.registerErrorTint.innerHTML = "";
			sign.dom.regitarAffirmPassword.className = "sign-regitar-affirm-password success-active";
		}
	}

	sign.dom.regitarTel.onblur = function () {
		if (!sign.dom.regitarTel || !sign.dom.regitarTel.value) {
			sign.dom.registerErrorTint.innerHTML = "请输入手机号码！";
			sign.dom.regitarTel.className = "sign-regitar-tel error-active";
			return;
		} else if (!sign.isMobile(sign.dom.regitarTel.value)) {
			sign.dom.registerErrorTint.innerHTML = "请输入正确的手机号码！";
			sign.dom.regitarTel.className = "sign-regitar-tel error-active";
		} else {
			sign.dom.registerErrorTint.innerHTML = "";
			sign.dom.regitarTel.className = "sign-regitar-tel error-active success-active";
		}
	}

	sign.dom.regitarNote.onblur = function () {
		if (!sign.dom.regitarNote || !sign.dom.regitarNote.value) {
			sign.dom.registerErrorTint.innerHTML = "请填写短信验证码";
			sign.dom.regitarNote.className = "sign-regitar-note sign-regitar-moddle-btn error-active";
			return;
		} else {
			sign.dom.registerErrorTint.innerHTML = "";
			sign.dom.regitarNote.className = "sign-regitar-note sign-regitar-moddle-btn success-active";
		}
	}

	sign.dom.regitarVerificatCode.onblur = function () {
		if (!sign.dom.regitarVerificatCode || !sign.dom.regitarVerificatCode.value) {
			sign.dom.registerErrorTint.innerHTML = "请填写验证码";
			sign.dom.regitarVerificatCode.className = "sign-regitar-verificat-code sign-regitar-moddle-btn error-active";
			return;
		} else {
			sign.dom.registerErrorTint.innerHTML = "";
			sign.dom.regitarVerificatCode.className = "sign-regitar-verificat-code sign-regitar-moddle-btn success-active";
		}
	}

	// sign.ajax("/validate/validatePhone.html?random="+Math.round(Math.random()*100),function (call) {
		
	// });
}

sign.ajax = function (url,call) { 
	//先声明一个异步请求对象
	var xmlHttpReg = null;
		if (window.ActiveXObject) {//如果是IE
			xmlHttpReg = new ActiveXObject("Microsoft.XMLHTTP");
	} else if (window.XMLHttpRequest) {
		xmlHttpReg = new XMLHttpRequest(); //实例化一个xmlHttpReg
	}

	//如果实例化成功,就调用open()方法,就开始准备向服务器发送请求
	if (xmlHttpReg != null) {
		xmlHttpReg.open("get", url, true);
		xmlHttpReg.send(null);
		xmlHttpReg.onreadystatechange = doResult; //设置回调函数
	}

	function doResult() {
		if (xmlHttpReg.readyState == 4) {//4代表执行完成
			if (xmlHttpReg.status == 200) {//200代表执行成功
			   //将xmlHttpReg.responseText的值赋给ID为resText的元素
			   call(JSON.parse(xmlHttpReg.responseText));
			   sign.loginResponseText = xmlHttpReg.responseText;
			}
		}
	}
}

// 是否是邮箱
sign.isEmail = function (email) {
	var remail = /^([\w-_]+(?:\.[\w-_]+)*)@((?:[a-z0-9]+(?:-[a-zA-Z0-9]+)*)+\.[a-z]{2,6})$/i;
	return remail.test(email);
}

// 是否是手机
sign.isMobile = function (m) {
 	return /^0?1[3|4|5|8][0-9]\d{8}$/.test(m);
}

// 初始化
sign.init = function () {
	sign.getDom();
	sign.tab();
	sign.login();
	sign.regitar();
}

sign.init();
