# -<style>
/*Общий Контейнер*/
.wheel_wrp {
 width: inherit;
 box-sizing: border-box;
 box-shadow: 0 0 0 10px #ffffff, 0 0 15px 3px #ababab;
 position: relative;
 overflow: hidden;
 border-radius: 50%;
}
/*Ось колеса в центре*/
.wheel_wrp:after {
 content: "";
 width: 35px;
 height: 35px;
 border-radius: 50%;
 background-color: #fff;
 position: absolute;
 left: 50%;
 top: 50%;
 transform: translate(-50%, -50%);
}
/*Оформление сектора*/
.wheel_sector{
 width: 0px;
 height: 0px;
 border-style: solid;
 position: absolute;
 top: 0;
 display: flex;
 justify-content: center;
 align-items: center;
}
/*Оформление разделительных линий*/
.wheel_line {
 background-color: #fff; /*Цвет линии*/
 transform: rotate(0deg);
 height: 4px;
 position: absolute;
 z-index: 99;
}
/*Оформление текста*/
.wheel_textcont {
 position: absolute;
 top: 0;
 z-index: 99999;
 display: flex;
 flex-direction: column-reverse;
 justify-content: flex-end;
 align-items: center;
 font-family: 'Montserrat',ExtraLight;
 font-weight: 300;
 color: #FFFFFF;
 font-size: 10px;
 line-height: 1.4;
 text-align: center;
}
.wheel_textcont div {
 transform: rotate(-90deg);
 margin-top: 35px;
 width: 120px;
}
/*Иконка*/
.wheel_textcont img {
 width: 0px;
 transform: rotate(-90deg);
 margin-top: 0px;
}
/*Адаптив текст + иконка*/
@media screen and (max-width:360px){
.wheel_textcont div {display: none} 
.wheel_textcont img {
 width: 0px;
 margin-top: 0px;
}
}
@media screen and (max-width:360px){
.wheel_textcont img {
 width: 0px;
 margin-top: 0px;
}
}
.spin_wheel , .open-wheel , .close-wheel{ cursor:pointer}
.spin_wheel { transition: all 0.2s linear}
.spin_wheel:hover {filter: sepia(1)}
.form_noactive{
 opacity:0.4;
 pointer-events:none;
}
.wheel_form .t-input-subtitle,
.wheel_form input.t-input{
 text-align: center;
}
/*Показать блок*/
.wheel-pos{
 position:fixed;
 width:100%;
 top:0;
 z-index:999;
}
.wheel-time{transition: transform 0.5s ease-in-out}
.slide-wheel{transform: translateX(-100%)}
.wheel-open-body{overflow:hidden}
</style>

<script>
$( document ).ready(function() {
 
//Список секторов : Цвет - Текст - ссылка на иконку 
let wheelOption = [
 ['#ffafcc' , 'кэшбек<br><strong>5 %</strong>' , 'https://static.tildacdn.com/tild6363-3135-4163-b762-326436666664/free-icon-discount-2.png'],
 ['#cdb4db' , 'консультация <br><strong>от академии "Velvet House"</strong>' , 'https://static.tildacdn.com/tild3862-3335-4039-b561-343662353333/premium-icon-consult.png'],
 ['#ffafcc' , 'кэшбек<br><strong>10 %</strong>' , 'https://static.tildacdn.com/tild6363-3135-4163-b762-326436666664/free-icon-discount-2.png'],
 ['#cdb4db' , 'курс "Хочу<br><strong>стать музой"</strong>' , 'https://static.tildacdn.com/tild3230-3333-4437-b230-663861393135/free-icon-brainstorm.png'],
 ['#ffafcc' , 'кэшбек<br><strong>15 %</strong>' , 'https://static.tildacdn.com/tild6363-3135-4163-b762-326436666664/free-icon-discount-2.png'],
 
 ['#cdb4db' , 'пробники<br><strong>"Velvet House"</strong>' , 'https://static.tildacdn.com/tild3639-3630-4932-a437-653063343133/free-icon-oil-522043.png'],
 ['#ffafcc' , 'курс<br><strong>"Богатства и изобилия"</strong>' , 'https://static.tildacdn.com/tild3862-3335-4039-b561-343662353333/premium-icon-consult.png'],
 ['#cdb4db' , 'скидка 50 %<br><strong>на объем 50 мл</strong>' , 'https://static.tildacdn.com/tild6363-3135-4163-b762-326436666664/free-icon-discount-2.png'],
 
];

//Создаём элемент в Zero
let sector = wheelOption.length;
$('.wheelfortune').append('<div class="wheel_wrp"></div>');
$('.wheelfortune').html($('.wheel_wrp'));
let wheelRec = $('.wheelfortune').closest('.t-rec');
wheelRec.addClass('wheel-pos slide-wheel');
$('.close-wheel').fadeOut(200);
//Кол-во оборотов до остановки
let maxRevolution = 20;
//Время вращения
let spinTime = 5;
let spinFinish = false;
let resizeTxt;
let diameter;

//Создание колеса
function creatingWheel(){
//Диаметр колеса
diameter = $('.wheel_wrp').width();
//Угол сектора
let angle = Number(((360*Math.PI)/(sector*180)).toFixed(3));
//Катет сектора
let sectorHalfWidth = 0.5*diameter*Math.tan(angle/2);
//Заполняем сектора
for (let i=0; i<sector;
i++){
$('.wheel_wrp').append('<div class="wheel_sector bg-sector'+i+'"></div><div class="wheel_line line-sector'+i+'"></div><div class="wheel_textcont text-sector'+i+'"><div></div><img src='+wheelOption[i][2]+'></div>');
//Формируем угол поворота и задаём цвет
$('.bg-sector'+i+'').css({
 "transform":"rotate("+(360*i/sector+90)+"deg)",
 "border-color": ""+wheelOption[i][0]+" transparent"
});
//Отрисовка границ сектора
$('.line-sector'+i+'').css({
 "width" : diameter/2+"px",
 "transform" : "rotate("+((360*i/sector)+(180*(sector-1))/sector)+"deg)",
 "height":"4px",
 "top" : "calc(50% - 2px)",
 "transform-origin" : diameter/2+"px 2px"
});
//Расставляем текст
$('.text-sector'+i+' div').html(wheelOption[i][1]);
// Формируем угол для текста
$('.text-sector'+i+'').css({"transform":"rotate("+(360*i/sector+90)+"deg)"});
};
//Форма сектора
$('.wheel_sector').css({
 "border-width": (diameter/2)+"px "+sectorHalfWidth+"px 0",
 "transform-origin": ""+sectorHalfWidth+"px "+(diameter/2)+"px",
 "left":((diameter/2)-sectorHalfWidth)+"px"
});
//Форма текста
$('.wheel_textcont').css({
 "width": (diameter/2)+"px",
 "height": (diameter/2)+"px",
 "transform-origin": ""+(diameter/4)+"px "+(diameter/2)+"px",
 "left":((diameter/2)-(diameter/4))+"px",
});
}; creatingWheel();

//Вращение колеса
function spinWheel(deg){
 $('.wheel_wrp').css({
 "height": diameter+"px",
 "transform" : "rotate("+deg+"deg)",
 "transition": "transform "+spinTime+"s cubic-bezier(0, 0.76, 0.5, 1.01)"
 });
};spinWheel(0);
//Случайный сектор 
function getRandomInRange(min, max) {
 return Math.floor(Math.random() * (max - min + 1)) + min;
}
let finalSector = -1;
//При нажатии на кнопку вращения
$('.spin_wheel .tn-atom').click(function(e) {
 //Получаем финальный сектор 
 finalSector = getRandomInRange(0, sector-1);
 //Поворачиваем колесо
 spinWheel(maxRevolution*360 + (finalSector*360)/sector );
 if (finalSector==0) finalSector=sector;
 //Выводим текст и картинку
 setTimeout(function(){
 let prizTxt = wheelOption[sector-finalSector][1];
 let prizeImg = wheelOption[sector-finalSector][2];
 spinFinish = true;
 finalStep('Ваш приз:<br>'+prizTxt, prizeImg);
 //Сохраняем результат
 let finalResult = {
 text: prizTxt , 
 img: prizeImg ,
 sect:finalSector
 };
 localStorage.setItem('_code_w', JSON.stringify(finalResult));
 }, spinTime*1000);
});
 
//Проверка прошлого вращения
let prevResult = localStorage.getItem('_code_w');let prevList;
let prevSend = localStorage['_wh_send'];
if (prevResult) {
 prevList = JSON.parse(prevResult);
 spinFinish = true;
 $('.wheel_form button').attr('type','submit');
 let preffTxt = '<u>Ваш прошлый приз</u><br>';
 if(prevSend){
 preffTxt = '<u>Заявка уже была отправлена</u><br>';
 blockForm();
 spinFinish = false;
 };
 finalStep(preffTxt+prevList.text, prevList.img);
 spinWheel( (prevList.sect*360)/sector );
};

//Блокируем форму
function blockForm(){
if(!prevResult || prevSend ){
$('.wheel_form').addClass('form_noactive');
setTimeout(function(){$('.wheel_form button').attr('type','button')}, 3500);
};
};blockForm();

//Отправка формы
$('.wheel_form').delegate(".t-submit", "click", function(){
 setTimeout(function(){
 if ( $('.wheel_form .t-form').hasClass("js-send-form-success")){
 console.log(4654);
 localStorage['_wh_send'] = 'sf';closeWheel();blockForm();
 };
 }, 1000);
});

//Отрисовка финального шага
function finalStep(prizTxt, prizeImg){
 //Удаляем кнопку
 $('.spin_wheel').remove(); 
 $('.present_text .tn-atom').html(prizTxt);
 $('.present_img img').attr({
 'data-original':prizeImg,
 'src':prizeImg
 });
 //Открываем форму
 if(spinFinish){ 
 $('.wheel_form').removeClass('form_noactive');
 $('.wheel_form button').attr('type','submit');
 };
 //Заполняем поле
 setTimeout(function(){
 resizeTxt = prizTxt.replace(/<\/?[^>]+>/g,' ');
 $('input[name="sector_prize"]').val(resizeTxt);
 }, 2500);
};
//Изменение размера окна
$( window ).resize(function() {
clearTimeout(window.resizedFinished); 
window.resizedFinished = setTimeout(function(){
 $('.wheel_wrp').empty();creatingWheel();
 if(finalSector>=0) { spinWheel( (finalSector*360)/sector )
 }else if (prevResult) {
 spinWheel( (prevList.sect*360)/sector );
 }else{spinWheel();};
 setTimeout(function(){ 
 if(!spinFinish){ $('.wheel_form button').attr('type','button');
 }else{ $('input[name="sector_prize"]').val(resizeTxt) };
 }, 3500); 
}, 500);
}); 

//Показать блок
let firstOpen = true;
$('.open-wheel').click(function(){
 $('body').addClass('wheel-open-body');
 wheelRec.addClass('wheel-time').removeClass('slide-wheel');
 if(firstOpen && !prevResult ) {spinWheel(360); firstOpen = false};
 setTimeout(function(){
 $('.close-wheel').fadeIn(200);
 t_lazyload_update();
 }, 550);
});

//Скрыть блок
function closeWheel(){ $('body').removeClass('wheel-open-body');$('.close-wheel').fadeOut(0); wheelRec.addClass('slide-wheel');};
$('.close-wheel').click(function(){closeWheel();});
$(document).keydown(function(eventObject){
 if(eventObject.which == 27 && !wheelRec.hasClass('slide-wheel')){closeWheel()};
});
});
</script>
