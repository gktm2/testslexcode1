// ==UserScript==
// @name         SakuraKeepaLinkerMod for Sleipnir
// @description  for amazon
// @author       shyntom
// @include        https://www.amazon.co.jp/*/dp/*
// @include        https://www.amazon.co.jp/*/gp/*
// @include        https://www.amazon.co.jp/dp/*
// @include        https://www.amazon.co.jp/gp/*
// @require api
// @require api-notification
// @version      0.7
// @history:jp   0.1 dousasinai
// @history:jp   0.3 sleipnirでdousakakunin
// @history:jp   0.4 sleep() 最後の挿入で、おそらく前の操作が終わっておらず、リンクが挿入されない問題に対処
// @history:jp   0.41 matchしない際に代入にエラーとなることを修正　sleipnir動作
// @history:jp   0.5 matchの条件がうまく動作しないため、10桁コードでhitさせる
// @history:jp   0.6 ステータス表示,button
// @history:jp   0.7 Sleipnirで謎に再描画されない。；のみの行があると再描画される。。。
// ==/UserScript==

(function() {
	let HowmanytimesInteerval = 0;
	let objInterval;

	let browser = "Sleipnir";
	/* chrome,sleipnir切り替え */
	try{
		/* tryの中で変数宣言してはダメ */
		/*let Browser = "Sleipnir";*/
		SLEX_locale;
	}catch(e){
		browser = "Chrome";
	}

	function addIcon() {
        let Statusico = document.body.appendChild(document.createElement('div'));
       	Statusico.id = 'SakuraStatyusIcon';
		Statusico.style.bottom = '0px';
		Statusico.style.left = '10px';
		Statusico.style.height = '25px';
		Statusico.style.opacity = 0.7;
		Statusico.style.position = 'fixed';
	}

	function inertLinkbtn(a){
		if(browser.indexOf('Chrome') != -1) {
			if(window.innerWidth > screen.availWidth){
				if(document.querySelectorAll("#booksTitle").length > 0) {
					document.querySelectorAll("#booksTitle")[0].append(a);
				}else if (document.querySelectorAll("#titleSection").length > 0) {
					document.querySelectorAll("#titleSection")[0].append(a);
				}
			}else{
				if(document.getElementById("title") != null) {
					document.getElementById("title").append(a);
				}else{
				}
			}
		}else if (browser =="Sleipnir"){
			if (document.getElementById("title") != null) {
				document.getElementById("title").append(a);
			}else{
                changeStatsIco("ractangle","insert pointが見つからない","red");
			}
		}
	}

	function changeStatsIco(state,text,color){
		let StatusIco = document.getElementById("SakuraStatyusIcon");
		switch (state) {
			case 'square':
                StatusIco.textContent = text;
				StatusIco.style.textAlign = 'center';
				StatusIco.style.fontSize = '15pt';
                StatusIco.style.width = '30px';
				StatusIco.style.backgroundColor = color;
                StatusIco.style.color = "white";
                StatusIco.style.zIndex = '9999';
				break;
			case 'ractangle':
                StatusIco.textContent = text;
				StatusIco.style.textAlign = 'center';
				StatusIco.style.fontSize = '15pt';
				StatusIco.style.backgroundColor = color;
                StatusIco.style.color = "white";
                StatusIco.style.zIndex = '9999';
                break;
		}
	}

	function extractItemID() {
		/* matchがhitしないとき、[1]など指定しているとエラーになる*/
		/*console.log();
		.*?  最短一致
		console.log(location.href.match(/dp\/(.*?)\//)[1]);*/

		let asin = "";
		try{
			asin = location.href.match(/dp\/([a-zA-Z0-9]{10})/)[1];
            console.log(asin);

		}catch(e){
            changeStatsIco("ractangle","noASIN","red");
		};
        changeStatsIco("ractangle","asin : "+ asin,"blue");
		return asin;
	}

	function InsertObserver() {
		HowmanytimesInteerval++;
		if(HowmanytimesInteerval < 3){
			if(document.getElementById("keepa") != null){
                changeStatsIco("ractangle","already inserted","gray");
				/* ↓がないと、Sleipnirで画面の再描画がされない。。。 */
				;
				clearInterval(objInterval);
			}else{
				injectLinks();
			}
		}else{
			/*alert("limited time");*/
            changeStatsIco("ractangle","END","gray");

			clearInterval(objInterval);
		}
	}

	function injectLinks() {
		let asin = extractItemID();
        let divcontainer = document.createElement("div");
        divcontainer.style.display = "flex";
        divcontainer.style.justifyContent="flex-end";

		let str = document.createTextNode("Price on Keepa");
		let a1 = document.createElement("div");
		a1.setAttribute("id","keepa");
		a1.style.border = "1px solid #999";
		a1.style.borderRadius = "3px";
		a1.style.margin = "4px";
		a1.style.padding = "7px";
		a1.style.backgroundColor = "rgb(202, 230, 252)";
		a1.appendChild(str);
		let a2 = document.createElement("a");
		a2.href = "https://keepa.com/#!product/5-" + asin;
		a2.target = "_blank";
		a2.appendChild(a1);

		let str2 = document.createTextNode("SakuraChecker");
		let b1 = document.createElement("div");
 		b1.setAttribute("id","sakura");
 		b1.style.border = "1px solid #999";
 		b1.style.borderRadius = "3px";
        b1.style.marginLeft = "40px";
 		b1.style.margin = "4px";
 		b1.style.padding = "7px";
 		b1.style.backgroundColor = "rgb(255, 235, 250)";
		b1.appendChild(str2);
		let b2 = document.createElement("a");
		b2.href = "https://sakura-checker.jp/search/" + asin + "/";
		b2.target = "_blank";
		b2.appendChild(b1);

        divcontainer.appendChild(a2);
        divcontainer.appendChild(b2);
		/*await new Promise(resolve => setTimeout(resolve, 300));*/
		inertLinkbtn(divcontainer);
	}

	window.onload = function() {
        addIcon();
        changeStatsIco("ractangle","start","green");
		injectLinks();
		objInterval = setInterval(InsertObserver, 5000);
	}
})();
