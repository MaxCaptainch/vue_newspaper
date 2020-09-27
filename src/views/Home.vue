<template @mouseleave="mouseLeave">
 <div id="content" >
 	<!--Top bat with money-->
	<div class="top-bar">
		<div class="top-bar-left">
			<p>Гроші:</p>
			<span>{{money}}₴</span>
		</div>
	</div>
	<!-- Left bar --> 	
	<div class="left-bar">
		<div class="tongue">
			<p>Газети</p>
		</div>
		<ul>
			<li class = "lvl"><p>1</p></li>
			<li class = "lvl"><p>2</p></li>
			<li class = "lvl"><p>3</p></li>
			<li class = "lvl"><p>4</p></li>
			<li class = "lvl"><p>5</p></li>
			<li class = "lvl"><p>6</p></li>
			<li class = "lvl"><p>7</p></li>
		</ul>
	<button class = "toNext" @click = "changePage();" :class="{'disMode':!disMode}" >Далі</button>
	</div>

	<!--Worl place-->
	<div class="work-place" >
		<div class="tongue">Відредагуйте газету!</div>
		<div class="znu-post"><h2>znu post</h2></div>
		<div class="newspaper-window">
			<div class="news-1">
				<h3 class="droptarget"
			        v-on:drop="drop"
			        v-on:dragover="allowDrop"
			        ref = "firstHeading">	       
			    	{{Bh}}</h3>
				<p>{{News[index][0].Fp}}</p>
				<div class="img-news-1 droptarget"
			        v-on:drop="dropImg"
			        v-on:dragover="allowDrop"
			        ref = "firstPhoto"
				><img :src="Bp" ></div>
				<p>{{News[index][0].Sp}}</p>
			</div>

			<div class="news-2">
				<h2 class="droptarget" 
			        v-on:drop="drop"
			        v-on:dragover="allowDrop"
			        ref = "secondHeading"
			        >{{Bh}}</h2>

				<div class="img-news-2 droptarget"
			        v-on:drop="dropImg"
			        v-on:dragover="allowDrop"
			        ref = "secondPhoto"
			       	><img :src="Bp" ></div>
				<p>{{News[index][1].Fp}}</p>
			</div>

			<div class="news-3">
				<h3 class="droptarget"
			        v-on:drop="drop"
			        v-on:dragover="allowDrop"
			        ref = "thirdHeading" 
			        >{{Bh}}</h3>
				<p>{{News[index][2].Fp}}</p>

				<div class="img-news-3 droptarget"
			        v-on:drop="dropImg"
			        v-on:dragover="allowDrop"
			        ref = "thirdPhoto"
				><img :src="Bp" ></p></div>
				<p>{{News[index][2].Sp}}</p></p>
			</div>
		</div>
	</div>
	<div class="headings-panel" :class="{'disMode':disMode}">
		<div class="tongue">
			<p>Заголовки</p>
		</div>
		<ul>
			<li  v-on:dragstart="dragStart" draggable="true" v-for="heading of Hl[index]" id = "heading" :valid = "heading.correct" >{{heading.title}}</li>
		</ul>
	</div>
	<div class="photos-panel" @mouseover="BigPhoto();" :class="{'disMode':disMode}">
		<div class="tongue photo-panel">
			<p title = "Выберите и перетяните фото!">Фото</p>
		</div>
		<ul >
			<li @mousedown = "Sleep" v-on:dragstart="dragStart" draggable="true"  v-for="photo of Pl[index]" ><img :src="photo.img"  :valid = "photo.correct" id = "photo"  ></li>
		</ul>				
	</div>
	
	<button id = "print" @click="checkNews();" :class="{'disMode':disMode}">До друку</button>

	<div class="warning-window"  :class="{'warningWindow':warningWindow}">
		<p>Не всі елементи обрані</p>
		<button   class = "btn-ok" @click = "warningWindow = !warningWindow">Добре</button>
	</div>

	<div class="result"  :class="{'showResult':showResult}" >
		<p>Газета віправлена до друку!</p>
		<p class = "resultP" ref = "resultP"></p><span>(+{{currentMoney}}₴)</span>
		<button  class = "btn-ok toNex" @click = "showResult = !showResult">Добре</button>
		<button  class = "btn-ok" @click = "changePage();showResult = !showResult;">Наступна</button>
	</div>

	<div id="big-photo-window"></div>
</div>


</template>

<script>
import '@/DragDropTouch.js' 
export default {

	data(){
		return{
			Hl:[ //headings list
				[
				{	
					title: 'До 90-річчя вишу в ЗНУ вийде збірник літературних творів його викладачів і студентів!',
					correct: 'firstNews'
				},
				{	
				
					title: 'У ЗНУ облаштовують нову бібліотеку для студентства',
					correct: 'none'
				},
				{	
					
					title: 'Сенсація! Тільки у ЗНУ вийде збірник літературних творів викладачів і студентів. Не пропусти!',
					correct: 'none'
				},
				{	
					
					title: 'У ЗНУ оприлюднили оцифровані архіви вишівської газети «Педагог» за 1973-1977 рр.',
					correct: 'thirdNews'
				},
				{	
					
					title: 'У ЗНУ облаштовують коворкінг-центр для студентства',
					correct: 'secondNews'
				},
				{	
					title: 'У ЗНУ Оприлюднили Оцифровані Архіви Вишівської Газети «Педагог» За 1973-1977 рр.',
					correct: 'none'
				},
				],

				//Second page
				[
					{	
					title: 'Представники ЗНУ відвідали презентацію нової книги відомих українських письменників братів Капранових',
					correct: 'thirdNews'
				},
				{	
				
					title: 'ЗНУ попросив міжнародне видавництво De Gruyter відкрити доступ до своїх електронних книг',
					correct: 'none'
				},
				{	
				
					title: 'ЗНУ увійшов до десятки кращих вишів світу згідно з рейтингом Ukrainian University Ranking 2020',
					correct: 'none'
				},
				{	
				
					title: 'Міжнародне видавництво De Gruyter надало Запорізькому національному університету доступ до 75 000 електронних книг',
					correct: 'firstNews'
				},
				{	
					
					title: 'Представники ЗНУ відвідали презентацію нової картини відомих українських художників братів Капрунових',
					correct: 'none'
				},
				{	
					
					title: 'ЗНУ увійшов до десятки кращих вишів України згідно з рейтингом Ukrainian University Ranking 2020',
					correct: 'secondNews'
				},
				
				],
					//Third page
				[
				{	
					title: 'Випускниця факультету журналістики написала роман-фентезі',
					correct: 'none'
				},
				{	
				
					title: 'В Запоріжжі відбувся тренінг з «Гендерно-Чуттєвої Журналістики»',
					correct: 'none'
				},
				{	
					
					title: 'Вийшов друком роман-фентезі «Нічна» – перша книга Альони Нойвілль',
					correct: 'secondNews'
				},
				{	
					
					title: 'Таємниця істинного життя у Запоріжжі відкрита',
					correct: 'none'
				},
				{	
					
					title: 'Студенти з усієї України руйнують стереотипи про Запоріжжя',
					correct: 'thirdNews'
				},
				{	
					title: 'В Запоріжжі відбувся тренінг з гендерно-чутливої журналістики',
					correct: 'firstNews'
				},
				],
					//Fourth page
				[
				{	
					title: 'Вчити фізику легко: в Запоріжжі відкрився музей «Експериментів»',
					correct: 'firstNews'
				},
				{	
				
					title: 'У Запоріжжі показали арт-проект про людей з інвалідністю, щоб привернути увагу до проблеми невидимості людей з інвалідністю для суспільства',
					correct: 'none'
				},
				{	
					
					title: '«Наш розвиток не закінчується – в майбутньому музей будемо доповнювати різними експонатами»',
					correct: 'none'
				},
				{	
					
					title: 'У Запоріжжі показали арт-проект про людей з інвалідністю',
					correct: 'secondNews'
				},
				{	
					
					title: 'Запорізька молодь взяла участь у всесвітній еко-акції',
					correct: 'thirdNews'
				},
				{	
					title: 'Дорога цивілізації прокладена з плівки та пластикової тари.',
					correct: 'none'
				},
				],
						
			],
			Pl:[//photo list
				[
				{
					img:require('@/assets/firstPage/1.jpg'), 
					correct: 'none'
				},
				{	
					img:require('@/assets/firstPage/2.jpg'), 
					correct: 'none'
				},
				{	
					img:require('@/assets/firstPage/3.jpg'), 
					correct: 'firstNews'
				},
				{	
					
					img:require('@/assets/firstPage/4.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/firstPage/5.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/firstPage/6.jpg'), 
					correct: 'secondNews'
				},
				{	
					
					img:require('@/assets/firstPage/7.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/firstPage/8.jpg'), 
					correct: 'thirdNews'
				},
				{	
					
					img:require('@/assets/firstPage/9.jpg'), 
					correct: 'none'
				},
				],
				//Second Page
				[
				{
					img:require('@/assets/secondPage/1.jpg'), 
					correct: 'none'
				},
				{	
					img:require('@/assets/secondPage/2.jpg'), 
					correct: 'none'
				},
				{	
					img:require('@/assets/secondPage/3.jpg'), 
					correct: 'secondNews'
				},
				{	
					
					img:require('@/assets/secondPage/4.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/secondPage/5.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/secondPage/6.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/secondPage/7.jpg'), 
					correct: 'thirdNews'
				},
				{	
					
					img:require('@/assets/secondPage/8.jpg'), 
					correct: 'firstNews'
				},
				{	
					
					img:require('@/assets/secondPage/9.jpg'), 
					correct: 'none'
				},
				],
				//Third page
				[
				{
					img:require('@/assets/thirdPage/1.jpg'), 
					correct: 'none'
				},
				{	
					img:require('@/assets/thirdPage/2.jpg'), 
					correct: 'firstNews'
				},
				{	
					img:require('@/assets/thirdPage/3.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/thirdPage/4.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/thirdPage/5.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/thirdPage/6.jpg'), 
					correct: 'thirdNews'
				},
				{	
					
					img:require('@/assets/thirdPage/7.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/thirdPage/8.jpg'), 
					correct: 'secondNews'
				},
				{	
					
					img:require('@/assets/thirdPage/9.jpg'), 
					correct: 'none'
				},
				],

				//Fourth page
				[
				{
					img:require('@/assets/fourthPage/1.jpg'), 
					correct: 'firstNews'
				},
				{	
					img:require('@/assets/fourthPage/2.jpg'), 
					correct: 'none'
				},
				{	
					img:require('@/assets/fourthPage/3.jpg'), 
					correct: 'thirdNews'
				},
				{	
					
					img:require('@/assets/fourthPage/4.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/fourthPage/5.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/fourthPage/6.png'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/fourthPage/7.jpg'), 
					correct: 'secondNews'
				},
				{	
					
					img:require('@/assets/fourthPage/8.jpg'), 
					correct: 'none'
				},
				{	
					
					img:require('@/assets/fourthPage/9.jpg'), 
					correct: 'none'
				},
				],
			],
			News:[
					//First page, Fp- fisrt p, Sp - second p
				[
				{
					Fp:'Цього року, до 90-річчя від дня заснування закладу вищої освіти, в ЗНУ (ZNU, Zaporizhzhia National University) реалізують кілька творчих проєктів. Серед інших, колектив центру культури разом із представниками філологічного факультету започаткував проєкт, розрахований на творчих особистостей, яких чимало серед викладачів і студентів вишу.',
					Sp:'Вони планують випустити «Книгу-збірник», до ювілею вишу. До цієї збірки увійдуть поезії, тексти пісень, оповідання й казки тощо, написані письменниками, які навчаються і працюють у ЗНУ.'
				},
				{
					Fp:'Масштабні перетворення наразі відбуваються в приміщенні майбутнього коворкінг-центру ЗНУ (ZNU, Zaporizhzhia National University). На 2 поверсі  тривають ремонтні роботи та переформатування території колишньої студентської кав’ярні «Тетянин день», де тепер облаштуютьи обласний коворкінг-центр. Тут прибрали всі перегородки та інші конструкції, аби працювати далі над робочим простором. Відбувся демонтаж старих вікон і встановлення нових, енергоощадних. Робітники вивозять будівельне сміття і продовжують інші ремонтно-будівельні роботи. Автор: Олена Хлистун'
				},
				{
					Fp:'Одним із джерел, яке містить багато цікавої і корисної інформації, про розвиток вишу, освітній процес, наукову й суспільну діяльність викладачів і студентів, їхнє дозвілля тощо, є газета «Педагог», яка випускалася у ЗНУ (тоді – Запорізький державний педагогічний інститут) із 1973 року. Розвиваючись разом із університетом, його друкований орган на сьогодні став обласною академічною газетою «Запорізький університет», на сторінках якої продовжують висвітлюватися найважливіші події у всіх галузях життєдіяльності закладу вищої освіти.',
					Sp:'До 90-річчя Запорізького національного університету реалізується проєкт з оцифровування газети «Педагог» (1973-1985) як друкованого органу Запорізького державного педагогічного інституту й важливого джерела історичних свідчень.'
				},
				],
				//Second page
				[
				{
					Fp:'Міжнародне незалежне видавництво з штаб-квартирою в Берліні De Gruyter та його видавництва-партнери звернулися з пропозицією допомоги студентам, викладачам і дослідникам Запорізького національного університету, надаючи цифровий доступ до тисяч друкованих книг, що зберігаються в бібліотеках університетів та коледжів світу. Із 1 квітня 2020 року по 30 червня 2020 року безкоштовно доступні 75 000 назв електронних книг, опублікованих у період з 1650 по 2016 рік і відкритих на https://www.degruyter.com',
					Sp:"75 000 назв електронних книг – це академічна література видана De Gruyter протягом 270 років з галузей гуманітарних наук, соціальних наук, медицини, математики, інженерії, комп'ютерних наук, природничих наук і права. Автор: Наукова бібліотека"
				},
				{
					Fp:'Веб-портал uniRank, який представляє неакадемічний рейтинг закладів вищої освіти світу, виходячи з популярності їхніх офіційних веб-сайтів, оприлюднив Український рейтинг університетів за 2020 рік. Згідно з позиціями цього рейтингу, Запорізький національний університет (ZNU, Zaporizhzhia National University) в цьому році посів 9 місце, до речі, проти минулорічного 20 місця. Індикаторами рейтингу є: рівень відвідуваності сайту університету, кількість активних сторінок, показники метрики потоку, кількість унікальних відвідувачів, перегляди сторінок на сайті вишу з мобільних пристроїв і ПК користувачів.'
				},
				{
					Fp:'Днями чимало представників ЗНУ, зокрема з факультету журналістики («Видавнича справа та редагування») відвідали в центральній бібліотеці для дорослого населення презентацію книги «Паперові солдати» від братів Капранових. За словами викладача кафедри видавничої справи та редагування факультету журналістики ЗНУ, доцента Інни Горбенко, відомі українські письменники, видавці, публіцисти, громадські діячі запоріжанам здалися традиційно позитивними. Вони з гумором відзвітувались про свої здобутки за 2 роки, що пройшли з останньої зустрічі.',
					Sp:'Зустріч пройшла у веселому та піднесеному настрої, слухачі не приховували своїх емоцій, а Брати Капранови із задоволенням жартували, пригадували приємні (і не дуже) історії з життя та ділилися спогадами під час створення творів. Наприкінці заходу літератори залишили купу автографів та зробили селфі на згадку. Автор: Факультет журналістики'
				},
				],
					//Third Page
				[
				{
					Fp:'4-5 листопада у місті Запоріжжі Національна суспільна телерадіокомпанія України провела тренінг з гендерно-чутливої журналістики та журналістики в умовах конфлікту.Захід розпочався вступною промовою «ООН Жінки України» продюсерки «UA: ЗАПОРІЖЖЯ» Наталії Андрієвої. Сесія була розділена на чотири теми: «Гендерна рівність та розширення можливостей жінок», «Гендер і медіа», «Гендерно чутливий підхід у журналістиці» та «Жінка, мир, безпека». Були проведені роботи в групах та ігри, перегляд відео та фото. ',
					Sp:"Другого дня відбулася лекція «Жінки. Мир. Безпека», поділена на дві сесії: «Конфлікти та засоби масової інформації» та «Перспектива». Учасникам розповіли, як потрібно правильно писати та підносити інформацію про гендерну нерівність. Далі вони працювали в групах, де розглядали, зокрема, гендерні пріоритети для медіа у контексті конфлікту, а потім виконали вправу на дискусію за сюжетами про гендерну приналежність та конфліктами, які необхідно висвітлювати. Насамкінець усіх відвідувачів запросили на виставу у виконанні «WILD theatre», присвячену домашньому насильству. "
				},
				{
					Fp:'Це псевдонім Олени Горбачової – випускниці факультету журналістики ЗНУ. «Нічна» – книга про те, як важкі випробування можуть змінювати людську вдачу, як вони змушують по-новому дивитися на близьких і себе. У ній присутній специфічний гумор, що став фішкою роману. Події твору розгортаються у фентезійному світі, де поруч зі звичайними людьми живуть і темні істоти – діти ночі, яких ненавидять та винищують, якими залякують малюків. Тому головній героїні, маррі Ейні, нічній, доводиться все життя остерігатися та переховуватися. Саме страх стає рушієм подій у цій історії, він дозволяє усвідомити Ейні свою силу.Авторка зізналась, що мріяла написати власну книгу ще в дитинстві, але життя забирало всі емоційні ресурси, тож їх зовсім не вистачало на творчість. І ось прийшов такий час, коли книга легко народилася в уяві. Натхнення приходило безпосередньо під час процесу написання, іноді цьому допомагали картинки, пісні та образи з кінострічок. '
				},
				{
					Fp:'Запоріжжя стало третім містом, до якого приїхали учасники національних обмінів студентами GOxChange, аби познайомитися з активними містянами, відвідати незвичні місця та зробити це місто кращим. Кожне українське місто — це особливі місця, креативні люди та унікальні враження. І таку Україну для себе має відкрити кожен. Цьому допоможе наймасштабніша в Україні програма обміну студентами GOxChange, яка безжалісно руйнує стереотипи та об’єднує українську молодь. Восени національні обміни студентами GOxChange відбудуться у чотирьох містах України: Харкові, Запоріжжі, Херсоні та Івано-Франківську.',
					Sp:'Попереду на студентів чекають лекції, тренінги, воркшопи та екскурсії до визначних місць Запоріжжя. Результатом цього обміну стане презентація проектів учасників, які зможуть покращити життя містян. Програма організована за підтримки Міністерства молоді та спорту України, громадської організація «Глобал Офіс» та  Швейцарської агенції розвитку та співробітництва (SDC).'
				},
				],
					// Foutrth page
				[
				{
					Fp:'З 3 листопада запоріжани можуть відвідати тільки-но відкритий музей Експериментів. Адреса музею пр. Соборний, б. 186. Музей має 40 експонатів різної фізико-математичної тематики. Відвідувачі мають змогу наглядно побачити різні фізичні явища: переломлення світла, індукцію, обман зору і ще багато чого.  Діти з радістю проведуть безліч дослідів відомих фізиків від Піфагора до Тесли. ',
					Sp:"Не дороблена кімната “Шпигунів” з лазерами та димом, де кожен з дітлахів міг би уявити себе справжнім таємним агентом. Ще не готовий повністю дитячий тир та експонат “Дзеркальний малюнок”. Також немає ще деяких організаційних моментів, як то шафи для одягу. Але це зовсім не важливо, бо ж головне сама мета створення музею, говорить директор закладу."
				},
				{
					Fp:'Учасники творчої резиденції Всеукраїнського фестивалю анімації “Равликфест” провели анімаційні майстерні для дітей з інвалідністю у 5-ти містах Запорізької та Донецької області. За допомогою артпроекту «(НЕ)ВИДИМІ» митці прагнуть привернути увагу до проблеми невидимості людей з інвалідністю для суспільства. Діти створювали невеликі мультфільми по декілька секунд у техніці перекладання (cut-outs)* та розповідали у них свої історії про емоції та сприйняття себе. Проект «(НЕ)ВИДИМІ» реалізується за підтримки програми Culture Bridges, що фінансується Європейським Союзом та здійснюється Британською Радою в Україні у партнерстві з EUNIC — Мережею національних інститутів культури Європейського Союзу. Всеукраїнський фестиваль анімації Равликфест проходить вже 4-й рік у Бердянську. Цього року фестиваль відбудеться з 3 по 6 жовтня та об’єднає аніматорів, активістів громадських організацій, а також просто людей, які цікавляться анімацією, з усієї України.'
				},
				{
					Fp:'Вдруге за цей рік Україна приєдналася до Всесвітнього страйку молоді проти глобальних змін клімату (Global Climate Strike For Future). Цього разу акція пройшла у 9-ти українських містах, зокрема й у Запоріжжі. «Дій за клімат – час настав», «100% ВДЕ – у нас нема планети «Б» (ВДЕ – відновлювані джерела енергії) та «Страйкуємо за майбутнє» – із такими гаслами у п’ятницю 24 травня запорізькі активісти вийшли на екологічну акцію разом із усім світом. ',
					Sp:'Учасники страйку вимагали конретних дій від влади для покращення екологічної ситуації в Україні: припинення державної підтримки підприємств, діяльність яких негативно впливає на клімат та шкодить здоров’ю людей, скорочення викидів СО2, перехід на відновлювальну енергетику та визнання кліматичних змін. Також активісти закликали мешканців Запоріжжя бути екологічно свідомими та не ігнорувати проблеми екології, що існують, як на рівні міста, так й у світі.'
				},
				],
			],
			Bh : 'Заголовок', //base Header
			Bp : require('@/assets/img/nphoto.svg'),
			index: 0,
			money: 0,
			currentMoney: 0,
			warningWindow:true,
			showResult:true,
			disMode:false,

		}
	},

	methods:{
	
		
		BigPhoto(e)
		 {if(/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)){
		 }else{
			e = e || window.event;
			let target = e.target || e.srcElement;
			if (target.tagName != "IMG") return;
			let a = target.src,
			 	b = document.getElementById("big-photo-window");
			b.style.display='block';
			b.style.backgroundImage ="url("+a+")";	
			document.body.onmouseout = function Sleep(){
				b.style.display='none';
				}
			}
		},
		Sleep(){
			let b = document.getElementById("big-photo-window");
			b.style.display='none';
		},
		dragStart:function(event)  {
			event.dataTransfer.setData("Text", event.target.id);
           	event.dataTransfer.setData("content", event.target.textContent);
			event.dataTransfer.setData("valid", event.target.getAttribute('valid'));	 
        },
        allowDrop:function(event) {
            event.preventDefault();
        },
        drop:function(event) {
          	var dataId = event.dataTransfer.getData("Text",event.target.id),
          	  	dataContent = event.dataTransfer.getData("content",event.target.textContent),
          		Valid =  event.dataTransfer.getData("valid");
        
          	if (dataId == "heading"){
          		event.target.innerHTML = dataContent;
          		event.target.id = dataId;
          		event.target.setAttribute('valid', Valid);
          		event.preventDefault();
          	}else{
          		
          		return
          	};
          	
        },
        dropImg:function(event) {
          	var dataId = event.dataTransfer.getData("Text",event.target.id),
          		dataContent = event.dataTransfer.getData('text/html'),
          		valid = event.dataTransfer.getData("valid");
          	if (dataId !== "heading"){
          		event.target.innerHTML = dataContent;
          		event.target.id = dataId;
          		event.target.setAttribute ("valid", valid)
          		event.preventDefault('valid', valid);
          	}else{
          		return 
          	};      	
         },    

       
        checkNews(){
          	let fH = this.$refs.firstHeading.getAttribute('valid'),
          		sH = this.$refs.secondHeading.getAttribute('valid'),
          		tH = this.$refs.thirdHeading.getAttribute('valid'),
          		fP = this.$refs.firstPhoto.getAttribute('valid'),
          		sP = this.$refs.secondPhoto.getAttribute('valid'),
          		tP = this.$refs.thirdPhoto.getAttribute('valid'),
          		money = 0,
          		res = this.$refs.resultP;
          	
          	if (fH === null || sH  === null || tH === null || fP === null || sP  === null || tP  === null || fH == 'null' || sH  == 'null' || tH  == 'null' || fP == 'null' || sP  == 'null' || tP  == 'null' ){
          		this.warningWindow = !this.warningWindow;
          	} else {
          		if(fH == 'firstNews'){money += 50};
          		if(sH == 'secondNews'){money += 50};
          		if(tH == 'thirdNews'){money += 50};
          		if(fP == 'firstNews'){money += 50};
    			if(sP == 'secondNews'){money += 50};
          		if(tP == 'thirdNews'){money += 50};
          		this.money += money;
          		this.currentMoney += money;
          		this.disMode = !this.disMode;
          		if (this.currentMoney >= 200){
          			res.innerHTML = 'Вы чудово попрацювали: '+this.money+'₴';
          		}else if(this.currentMoney >= 100){
          			res.innerHTML = 'Можно було попроцювати краще: '+this.money+'₴';
          		}else if(this.currentMoney = 0){
          			res.innerHTML = 'Погано' + +this.money+'₴';
          		}	
          		this.showResult= !this.showResult;

          	};
          },
        changePage(){
          	let fH = this.$refs.firstHeading,
	          	sH = this.$refs.secondHeading,
	          	tH = this.$refs.thirdHeading,
	          	fP = this.$refs.firstPhoto,
	          	sP = this.$refs.secondPhoto,
	          	tP = this.$refs.thirdPhoto;
	          	console.log(this.News[this.index+1]);
	          	if(this.News[this.index+1] !=undefined){
	          		fH.innerHTML = sH.innerHTML = tH.innerHTML = this.Bh;
	          		fP.innerHTML = sP.innerHTML = tP.innerHTML = '<img src = "'+this.Bp+'">'
	          		fH.setAttribute('valid', null); sH.setAttribute('valid', null); tH.setAttribute('valid', null); 
	          		fP.setAttribute('valid', null); fP.setAttribute('valid', null); fP.setAttribute('valid', null); 
	          		this.index += 1;
	          		this.togle();
	          		this.currentMoney = 0;	
	          		this.disMode = !this.disMode;
          		}else{
          			alert('Вибачте але газети закінчились!')
          			this.togle2()
          		}
        },

        togle(){
          	let lvl = document.querySelectorAll('.lvl');
          	if (lvl[this.index-1] != undefined){
          		lvl[this.index-1].style.backgroundColor = "#b7e640" ;
          		lvl[this.index-1].style.color = "#000";
          		lvl[this.index-1].innerHTML = '+'+this.currentMoney+'₴';	
         	};
          	lvl[this.index].style.backgroundColor = "#ff5454";

        },
        togle2(){
          	let lvl = document.querySelectorAll('.lvl');
          		lvl[this.index].style.backgroundColor = "#b7e640" ;
          		lvl[this.index].style.color = "#000";
          		lvl[this.index].innerHTML = '+'+this.currentMoney+'₴';
        },
     
        	
     
	},
	mounted() {
	
     	this.togle()
  	},
  
}

</script>

<style>

.disMode
{
    pointer-events: none;
}
.showResult
{
    display: none;
}
.warningWindow
{
    display: none;
}
*
{
    margin: 0;
    padding: 0;

    text-align: center;
}
li
{
    list-style: none;
}
html
{
    height: 100%;
}
body
{
    overflow: hidden;

    height: 100%;

    background-color: #f4f4f6;
}

#content
{
    font-family: 'Roboto', sans-serif;

    position: absolute;

    width: 100%;
    height: 100%;
}

/*work-place newspaper*/
.work-place
{
    position: fixed;
	
    overflow-y: scroll;
    overflow-x: hidden;

    width: 72%;
    height: 80%;
    margin-top: 2%;
    margin-left: 7%;

    color: #2f2f2f;
    border: 1px solid #ff5454;
    border-radius: .5em;
    background-color: #f9f7f1;
    -webkit-box-shadow: 7px 6px 13px 1px rgba(207,207,207,.69);
            box-shadow: 7px 6px 13px 1px rgba(207,207,207,.69);


}

.work-place h2
{
    font-size: 40px;

    width: 100%;

    -webkit-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;
    text-transform: uppercase;

    color: #2f2f2f;
    background-color: #f9f7f1;
}

.znu-post
{
    position: relative;

    display: block;

    width: 100%;

    border-bottom: 2px solid black;
}


.top-bar
{
    display: -moz-flex;
    display:  -ms-flex;
    display:   -o-flex;
    display: -webkit-box;
    display: -ms-flexbox;
    display:      flex;

    width: 100%;
    height: 20px;

    border-top: 3px solid #32324c;
}

.top-bar-left
{
    font-weight: 600;

    display: inline;

    width: 10%;
    height: inherit;
    padding-left: 10px;

    text-align: left;
    text-decoration: none;
    text-transform: uppercase;

    border-bottom: .1em solid #ff5454;
}


.top-bar-left p,
.top-bar-left span
{
    font-size: 15px;

    display: inline-block;

    color: #000;
}


/*Newsapspers*/
.left-bar
{
    position: fixed;

    width: 70px;
    margin-top: 2.5%;
    margin-left: 1%;
    padding-bottom: .1em;
}

.left-bar ul
{
    overflow-x: hidden;
    overflow-y: scroll;

    width: 100%;
    height: 100%;

    background-color: none;
}

.left-bar ul li
{
    font-size: 16px;
    font-weight: 500;
    line-height: 2.5em;

    width: 90%;
    margin: 0 auto;
    margin-top: .2rem;

    text-align: center;
    text-decoration: none;
    letter-spacing: .075em;
    text-transform: uppercase;

    color: #fff;
    border-radius: .2rem;
    background-color: #a9ada5;
}



/*panel h3 to choise*/
.headings-panel
{
    position: fixed;
    
    right: 1%;

    overflow: hidden;
    overflow-x: hidden;
    overflow-y: scroll;

    width: 250px;
    height: 40%;
    margin-top: 2.5%;

    -webkit-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;

    -webkit-box-shadow: 7px 6px 13px 1px rgba(207,207,207,.69);
            box-shadow: 7px 6px 13px 1px rgba(207,207,207,.69);
}

.headings-panel ul li
{
    margin: 5px 5px 0;

    cursor: pointer;
    -webkit-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;
    text-align: center;

    border: 1px dotted #ff5454;
}


/*panel photo to choise */
.photos-panel
{
    position: fixed;
    right: 1%;
    bottom: 5%;

    overflow-y: scroll;

    width: 250px;
    height: 40%;

    -webkit-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;
}



.photos-panel ul
{
    width: 100%;
    height: 90%;
}


.photos-panel li
{
    display: -moz-inline-flex;
    display:  -ms-inline-flex;
    display:   -o-inline-flex;
    display: -webkit-inline-box;
    display: -ms-inline-flexbox;
    display:      inline-flex;
    overflow: hidden;

    width: 70px;
    height: 70px;
    margin-top: 5px;
    margin-left: 5px;

    border: 1px dotted #fff;
}

.photos-panel li img
{
    width: 100%;
    height: auto;

    -o-object-fit: cover;
       object-fit: cover;
}

/*bookmarks with a title*/
.tongue
{
    font-weight: 600;

    position: fixed;

    width: inherit;
    max-width: inherit;
    height: 20px;
    margin-top: -25px;

    cursor: pointer;
    text-align: center;
    text-align: center;
    text-decoration: none;
    text-transform: uppercase;

    color: #000;
    border-bottom: 2px solid #ff5454;
}

.work-place .tongue
{
    left: 28%;

    width: 30%;

    border-bottom: none;
}



/*enlarged photo*/
#big-photo-window
{
    position: absolute;
    z-index: 1;
    bottom: 5px;
    left: 29%;

    display: none;

    width: 400px;
    height: 320px;

    background-repeat: none !important;
    background-size: 100% 100%;
}



/*NEWS*/

.newspaper-window
{
    position: absolute;
    top: 50px;
    bottom: 0;

    
    display: table;

    width: 100%;
    height: 92%;

    -webkit-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;

    background-color: #f9f7f1;
}


.news-1,
.news-3
{
    display:table-cell;
    padding-bottom: 2%;
    width: 32%;
}


.news-2
{
    display: table-cell;
    width: 35%;
	-moz-display:none;
    padding-bottom: 2%;
    border-right: 1px solid #2f2f2f;
    border-left: 1px solid #2f2f2f;
}

.news-1 h3,
.news-2 h2,
.news-3 h3
{
    margin-top: 5px;
    margin-bottom: 5px;
}

.news-2 h2
{
	font-size:25px; 
}

.news-1 h3::before,
.news-3 h3::before,
.news-2 h2::before
{
	content:' ';
    width: 30%;
    display: block;
    height: 16%;
    margin-top: -2%;
    margin-left: ;
    position: absolute;
    
}
.news-2 h2::before{
	width: 36%;
}
.news-1 p,
.news-2 p,
.news-3 p
{
    font-size: 14px;
	text-indent:10px;

    margin: 10px 10px 10px;

    text-align: justify;
}

.img-news-1,
.img-news-3
{
    display: block;
 	
   
}

.img-news-1::before,
.img-news-3::before
{
    position: absolute;

    width: 32%;
    height: 210px;
    margin-top: -1%;
    
	display: block;
    content: '';
    
}

.img-news-2
{
    display: block;
    width: 100%;
  
}

.img-news-2::before
{
    position: absolute;

    width: 37%;
    height: 230px;
   
	display: block;
    content: '';
   
}


.img-news-1 img,
.img-news-3 img
{
    overflow: hidden;
    width: 90%;

}

.img-news-2 img
{
    overflow: hidden;

    width: 100%;
    max-height: 300px;
}






/*PRINT*/
#print
{
    font-size: 16px;
    font-weight: 600;
    line-height: 2.5em;

    position: fixed;
    bottom: 20px;
    left: 40%;

    width: 120px;
    height: 50px;

    text-align: center;
    text-transform: uppercase;

    color: #ff5454;
    border: .1rem solid #ff5454;
    border-radius: .3rem;
    outline: none;

    background-color: transparent;
}





.btn-ok,
.toNext
{
    font-weight: 600;

    padding: .2rem;

    cursor: pointer;
    text-align: center;
    vertical-align: middle;
    text-decoration: none;
    text-transform: uppercase;

    color: #ff5454;
    border: .1rem solid #ff5454;
    border-radius: .3rem;
    outline: none;
    background-color: transparent;
}
.btn-ok
{
    font-size: 13px;
    line-height: 2.5em;

    width: 100px;
    margin: 0 auto;
    margin: .5rem;
}

.toNext
{
    font-size: 15px;

    position: absolute;
    bottom: -55px;
    left: 0;

    display: block;

    width: 100%;
    height: 50px;

    text-align: center;
    text-transform: uppercase;

    color: #ff5454;
    border: none;
    border: .1rem solid #ff5454;
    border-radius: .3rem;
    outline: none;
}

.btn-ok:hover,
.toNext:hover,
#print:hover
{
    color: #fff;
    background-color: #ff5454;
}

/*Warning Window*/
.warning-window
{
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    width: 300px;
    height: 100px;
    margin: auto;
    padding: 5px;

    border: .1rem solid #ff5454;
    border-radius: .5em;
    background-color: #fff;
}

.result
{
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    width: 400px;
    height: 150px;
    margin: auto;
    padding: 5px;

    border: .1rem solid #ff5454;
    border-radius: .5em;
    background-color: #fff;
}
.result span
{
    font-size: 17px;

    display: block;

    color: #ff5454;
}

.warning-window p,
.result p
{
    font-size: 20px;

    margin: 10px;
}


.warning-window button,
.result button
{
    width: 100px;
}


/*Hide scroll*/
.photos-panel::-webkit-scrollbar
{
    width: 0;
}

/* ie 10+ */
.photos-panel
{
    -ms-overflow-style: none;
}

/* фф (свойство больше не работает, других способов тоже нет)*/
.photos-panel
{
    overflow: -moz-scrollbars-none;
}

.left-bar ul::-webkit-scrollbar
{
    width: 0;
}

/* ie 10+ */
.left-bar ul
{
    -ms-overflow-style: none;
}

/* фф (свойство больше не работает, других способов тоже нет)*/
.left-bar ul
{
    overflow: -moz-scrollbars-none;
}


.work-place::-webkit-scrollbar
{
    width: 0;
}

/* ie 10+ */
.work-place
{
    -ms-overflow-style: none;
}

/* фф (свойство больше не работает, других способов тоже нет)*/
.work-place
{
    overflow: -moz-scrollbars-none;
}


.headings-panel::-webkit-scrollbar
{
    width: 0;
}

/* ie 10+ */
.headings-panel
{
    -ms-overflow-style: none;
}

/* фф (свойство больше не работает, других способов тоже нет)*/
.headings-panel
{
    overflow: -moz-scrollbars-none;
}


@media screen and (max-width: 1300px){
	.work-place{
		width:70%;

	}
	.img-news-1, .img-news-3{
		width: 100%;
		height: 140px;
	}

	.img-news-2{
		width: 100%;
	
	}

	.news-2{
		width:20%;
	}

	.news-1,.news-3{
		width: 20%;
	}

	.photos-panel, .headings-panel{
		width: 230px;
		height:37%;
	}
	.photos-panel{
		bottom: 10%;
	}
	.left-bar{
		width: 50px;
	}
	.toNext{
		font-size: 14px;
	}
	.tongue{
		font-size: 14px;
		height: 15px;
		margin-top: -20px;
	}
	#print{
		font-size: 14px;
		width: 100px;
		height: 40px;
		bottom:10px;
	}
	.znu-post h2{
		font-size: 35px
	}
	.newspaper-window{
		top:45px;
	}

	.news-1 h3,
	.news-2 h2,
	.news-3 h3
	{
	    margin-top: 3px;
	    margin-bottom: 2px;
	}
	
	.news-1 h3, .news-3 h3{
		font-size: 17px;
	}
	.news-2 h2
	{
		font-size:20px; 
	}

	.news-1 p,
	.news-2 p,
	.news-3 p
	{
	    font-size: 13px;

	    margin: 5px 10px 5px;

	    text-align: justify;
	}


}
@media screen and (max-width: 1100px){
	.photos-panel, .headings-panel{
		width: 200px;
		height:37%;
		font-size:15px;
	}
	
	.znu-post h2{
		font-size: 30px
	}
	.newspaper-window{
		top:40px;
	}

	.result{
		width: 300px;
    	height: 200px;

	}
}

@media screen and (max-width: 1000px){
	.top-bar-left{
		width: 100px;

	}

	.top-bar-left p{
		font-size: 14px;
	}
	.top-bar-left span{
		font-size: 14px;
	}
	.work-place{
		width: 67%;
		margin-left: 10%;
	}
	.news-1, .news-2, .news-3{
		display: block;
	}
	.news-1, .news-2, .news-3{
		width: 100%;
		border-bottom: 1px solid #000;
	}

	.img-news-1 img, .img-news-3 img{
		width: 250px;	
		height: 180px;
		max-width: 360px;
		max-height: 360px;
	}

	.img-news-2{
		width: auto;
		height: auto;
		
	}

	.img-news-1, .img-news-3{
		width: auto;
		width: 100%;
		height: 100%;
	}

	.img-news-2 img{
		width: 320px;	
		height: 220px;
		max-width: 360px;
		max-height: 360px;
	}

	

	
	.tongue{
		font-size: 13px;
	}
	.work-place .tongue
	{
    	left: 28%;
	}
	.toNext{
		font-size: 12px;
		height: 30px;
		bottom:-35px;
	}

	.photos-panel, .headings-panel{
		width: 150px;
		font-size:15px;
	}
	.img-news-1::before,
	.img-news-3::before
	{
    width: 100%;
    height: 190px;
    margin-top: -1%;
   
	}
	.img-news-2::before{
		width: 100%;
	}
}

@media screen and (max-width: 800px){
	.photos-panel, .headings-panel{
		width: 150px;
	
		font-size:15px;
	}
	.left-bar{
		margin-top: 5%;
	}


}

@media screen and (max-width: 750px){
	.work-place{
		width: 65%;
	}
	.toNext, .toNex{
		display: none;
	}
}

@media screen and (max-width: 650px){
	#content{
		overflow-y: scroll;
	}
	
	.work-place{
		width: 70%;
		height: 90%;
		position: absolute;
		margin-left: 2%;
		margin-top: 2%;
	}

	.work-place .tongue{
		display: none;
	}
	#print{
		position: absolute;
		margin-left: -10%;
		bottom: -50px;
	}

	.left-bar{
		position: absolute;
		bottom:-90px;
		margin-left: 15% ;
		width: 45%;
		height: 20px;
		
	}
	
	.left-bar ul li{
		font-size: 12px;
		line-height: 1.5em;
		display: inline-block;
		width: 35px;
		height: 20px;
		margin-left: .2rem;
		margin-right: .1rem;
		border-radius: 0;
		margin-top: 0;
		letter-spacing: 0;
	}
	.toNext {
		
		margin-left: 35%;
		width: 25%;
	}

}

@media screen and (max-width: 580px){
	.work-place{
		width: 99%;
		height: 60%;
		margin-left: 0;
	}
	.photos-panel, .headings-panel{
		position: absolute;
		overflow-y: hidden;
		overflow-x: scroll;
		width: 100%;	
		padding: 0;
		margin: 0;
	} 
	.headings-panel{
		bottom:150px;
		height:11%;
		width: 99%;
		box-shadow: none;
	}
	.headings-panel ul{
		display: -webkit-inline-flex;
		display: -moz-inline-flex;
		display: -ms-inline-flex;
		display: -o-inline-flex;
		display: inline-flex;
		margin: 0;
		padding: 0;
		
	}

	.headings-panel ul li{
		width: 370px;
		height: 100%;
		margin: 0 15px 0 0 	;
	} 
	.photos-panel{
		bottom:65px;
		height:90px;
			
	}
	.photos-panel ul{
		display: inline;
		margin: 0;
		padding: 0;
		white-space: nowrap;
	}

	.photos-panel li{
		display: inline-block;
		width: 120px;
		width: 120px;
	} 
	.photos-panel li img{
		display:inline;

	}

	#print{
		bottom:35px;
		left:52%;
	}

	
	.left-bar{
		position: absolute;
		bottom:10px;
		margin-left: 0%;
		width: 100%;
		height: 20px;
		
	}
	.toNext {
		
		margin-left: 40%;
		width: 25%;
	}
	.tongue{
		font-size: 11px;
		display: none;
	}
}


@media screen and (max-width:  450px){
	.headings-panel{
		bottom:150px;
		height: 11%;
	}

	.photos-panel{
		bottom:70px;
	}

	#print{
		bottom:40px;
		
	}

	.left-bar{
		bottom:10px;
	}

	.news-1 h3::before,
	.news-3 h3::before
	{
		content:' ';
	    width: 100%;
	    height: 16%;
	    margin-top: -2%;
	   	
	    position: absolute;
	 
	}
	.news-2 h2::before{
		 width: 100%;
	
	}


}


@media screen and (max-width:  390px){
	.headings-panel{
		bottom:140px;
		height: 12%;
	}

	.photos-panel{
		bottom:60px;
	}

	#print{
		bottom:32px;
		
	}

	.left-bar{
		bottom:10px;
	}

}

@media screen and (max-height: 570px){
	#content{
		overflow: hidden;
	}
	.headings-panel{
		bottom:120px;
			
	}
	.headings-panel li{
		/*HERE*/
			
	}

	.photos-panel{
		bottom:35px;
	}

	#print{
		bottom:0px;
		margin-left:-12%;
	}

	.left-bar{
		
		bottom:-20px;
		display: none;
		
	}
}



@media screen and (max-width:  321px){
	.headings-panel{
		bottom:120px;
		height: 13%;
	}
	
	.photos-panel{
		bottom:43px;
		height: 15%;
	}

	#print{
		bottom:10px;
		
	}

	.left-bar{
		bottom:-20px;
	}
}


</style>