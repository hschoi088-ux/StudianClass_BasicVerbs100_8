<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>기본동사100 스피드퀴즈</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            -webkit-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none;
            -webkit-touch-callout: none; -webkit-tap-highlight-color: transparent;
            word-break: keep-all; background-color: #f8fafc; font-family: system-ui, -apple-system, sans-serif;
        }
        .card { perspective: 1000px; }
        .card-inner {
            position: relative; width: 100%; height: 320px;
            transition: transform 0.6s; transform-style: preserve-3d; cursor: pointer;
        }
        .card.flipped .card-inner { transform: rotateY(180deg); }
        .card-front, .card-back {
            position: absolute; width: 100%; height: 100%;
            backface-visibility: hidden; display: flex; flex-direction: column;
            align-items: center; justify-content: center; border-radius: 1.5rem;
            padding: 2rem; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .card-back { transform: rotateY(180deg); }
        .active-tab { background-color: #4f46e5 !important; color: white !important; }
        .star-checked { color: #fbbf24 !important; fill: #fbbf24 !important; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .tab-section-label { font-size: 10px; font-weight: 900; color: #94a3b8; text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: 4px; margin-left: 4px; }
    </style>
</head>
<body oncontextmenu="return false">

    <div class="max-w-md mx-auto px-4 py-6">
        <header class="text-center mb-6">
            <h1 class="text-2xl font-black text-indigo-600 tracking-tight italic">기본동사100 스피드퀴즈</h1>
        </header>

        <div class="tab-section-label">Month 1 (W1-W4)</div>
        <div class="flex space-x-2 mb-4 overflow-x-auto pb-2 no-scrollbar font-bold text-sm">
            <button onclick="selectWeek('w1')" id="tab-w1" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W1</button>
            <button onclick="selectWeek('w2')" id="tab-w2" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W2</button>
            <button onclick="selectWeek('w3')" id="tab-w3" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W3</button>
            <button onclick="selectWeek('w4')" id="tab-w4" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W4</button>
            <button onclick="selectWeek('total1')" id="tab-total1" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W1-4 누적</button>
        </div>

        <div class="tab-section-label">Month 2 (W5-W8)</div>
        <div class="flex space-x-2 mb-6 overflow-x-auto pb-2 no-scrollbar font-bold text-sm">
            <button onclick="selectWeek('w5')" id="tab-w5" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100 active-tab">W5</button>
            <button onclick="selectWeek('w6')" id="tab-w6" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W6</button>
            <button onclick="selectWeek('w7')" id="tab-w7" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W7</button>
            <button onclick="selectWeek('w8')" id="tab-w8" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W8</button>
            <button onclick="selectWeek('total2')" id="tab-total2" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">W5-8 누적</button>
        </div>

        <div id="setup-screen" class="bg-white rounded-3xl p-8 shadow-xl border border-slate-100 text-center">
            <div id="selected-week-title" class="text-indigo-500 font-black text-xl mb-6 italic tracking-widest uppercase">WEEK 5 (Day 21-25)</div>
            <div class="space-y-4">
                <button onclick="startQuiz('mild')" class="w-full py-5 bg-emerald-500 text-white rounded-2xl font-black shadow-lg shadow-emerald-100 active:scale-95 transition">순한맛 시작</button>
                <button onclick="startQuiz('spicy')" class="w-full py-5 bg-orange-500 text-white rounded-2xl font-black shadow-lg shadow-orange-100 active:scale-95 transition">매운맛 시작</button>
            </div>
            <div class="mt-8 p-4 bg-slate-50 rounded-xl text-[11px] text-slate-400 text-left leading-relaxed">
                <p>• <strong>순한맛</strong>: 대표 + 교재1 (핵심 예문)</p>
                <p>• <strong>매운맛</strong>: 모든 교재 포함 (전체 랜덤)</p>
                <p class="mt-2 text-rose-400 font-bold italic">※ 각 모드당 랜덤 10문제가 출제됩니다.</p>
            </div>
        </div>

        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6 px-2">
                <span id="progress-text" class="px-3 py-1 bg-indigo-100 text-indigo-600 rounded-full text-xs font-bold">1 / 10</span>
                <button onclick="location.reload()" class="text-slate-400 font-bold text-xs uppercase">Exit ✕</button>
            </div>

            <div id="card-container" class="card" onclick="flipCard()">
                <div class="card-inner">
                    <div class="card-front bg-white border border-slate-100">
                        <span class="text-indigo-400 font-black text-[10px] tracking-widest mb-6 uppercase">Korean</span>
                        <p id="q-korean" class="text-xl font-bold leading-snug px-4 text-center"></p>
                        <p class="mt-10 text-slate-300 text-xs font-bold animate-pulse">카드를 터치하여 정답 확인</p>
                    </div>
                    <div class="card-back bg-indigo-600 text-white">
                        <button id="star-btn" onclick="toggleStar(event)" class="absolute top-6 right-6 text-white/40 transition-all">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-9 w-9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14l-5-4.87 6.91-1.01L12 2z"/>
                            </svg>
                        </button>
                        <span class="text-indigo-200 font-black text-[10px] tracking-widest mb-6 uppercase">English</span>
                        <p id="q-english" class="text-xl font-black leading-snug px-4 mb-8 text-center"></p>
                        <button onclick="event.stopPropagation(); playTTS();" class="w-20 h-20 bg-white/20 rounded-full flex items-center justify-center active:scale-90 transition shadow-inner">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                            </svg>
                        </button>
                        <div id="q-info" class="absolute bottom-8 text-[10px] text-indigo-300 font-bold uppercase tracking-widest italic"></div>
                    </div>
                </div>
            </div>
            <button id="next-btn" onclick="nextQuestion()" class="w-full mt-10 py-5 bg-indigo-600 text-white rounded-2xl font-black shadow-xl hidden active:scale-95 transition">다음 문제로 NEXT</button>
        </div>

        <div id="result-screen" class="hidden bg-white rounded-3xl p-6 shadow-xl border border-slate-100">
            <h2 class="text-center font-black text-xl text-slate-800 mb-8 tracking-tighter">퀴즈 완료 리포트</h2>
            <div id="starred-list" class="space-y-4 max-h-[400px] overflow-y-auto pr-2 custom-scroll"></div>
            <button onclick="location.reload()" class="w-full mt-10 py-5 bg-slate-900 text-white rounded-2xl font-bold active:scale-95 transition">메인으로 돌아가기</button>
        </div>
    </div>

    <script>
        const allData = [
            /* Day 1 ~ 35 (기존 데이터 완벽 유지) */
            {d:1, s:"대표", k:"반려동물 키우시나요?", e:"Do you have any pets?"},
            {d:1, s:"교재1", k:"내 조카는 거북이를 키운다.", e:"My nephew has a turtle."},
            {d:1, s:"교재1", k:"나는 시카고에 사는 친구들이 있다.", e:"I have some friends living in Chicago."},
            {d:1, s:"교재1", k:"남동생과 나는 우애가 돈독하다.", e:"My brother and I have a strong bond."},
            {d:1, s:"교재1", k:"내 생각은 달랐다.", e:"I had a different idea."},
            {d:1, s:"교재1", k:"친구들과 술래잡기하면서 놀던 즐거운 추억이 있다.", e:"I have some fond memories of playing tag with my friends."},
            {d:1, s:"교재2", k:"프로젝트가 끝나서 좋죠?", e:"Are you glad the project is over?"},
            {d:1, s:"교재2", k:"솔직히 시원섭섭해요. 일을 끝내서 홀가분하긴 한데, 팀이 그리울 것 같아요.", e:"Honestly, I have mixed feelings. I’m relieved we finished the work, but I’ll miss the team."},
            {d:1, s:"교재2", k:"음대 대신 의대를 선택한 거 맞지?", e:"You chose med school over music, right?"},
            {d:1, s:"교재2", k:"응, 힘든 결정이었어.", e:"Yeah, it was a tough decision."},
            {d:1, s:"교재2", k:"음악을 계속하지 않은 걸 후회해?", e:"Do you have any regrets about not pursuing music?"},
            {d:1, s:"교재2", k:"조금은 그렇지만, 되도록 생각 안 하려고 해", e:"A few, but I try not to think about it too much."},
            {d:1, s:"교재3", k:"그녀는 공포 영화보다는 영어 강의를 택할 겁니다.", e:"She would choose an English lecture over any horror movie."},
            {d:1, s:"교재3", k:"어떤 사람들은 가구를 살 때 가격보다 품질을 중시한다.", e:"Some people choose quality over price when buying furniture."},
            {d:1, s:"교재3", k:"어떻게 내가 아닌 그 사람을 선택할 수가 있어!", e:"I can’t believe you chose him over me!"},
            {d:1, s:"교재3", k:"장기적인 목표보다 눈앞의 즐거움을 선택하는 사람들이 있다.", e:"Some people choose short-term pleasure over long-term goals."},
            {d:2, s:"대표", k:"방금 간식을 먹었더니, 배가 별로 안 고파요.", e:"I just had a snack, so I’m not that hungry."},
            {d:2, s:"교재1", k:"나는 아침으로 주로 시리얼을 먹는다.", e:"I usually have cereal for breakfast."},
            {d:2, s:"교재1", k:"저는 스테이크 먹을게요.", e:"I’ll have the steak, please."},
            {d:2, s:"교재1", k:"오늘 점심으로 아무것도 안 먹었어요.", e:"I didn’t have anything for lunch today."},
            {d:2, s:"교재1", k:"출발하기 전에 뭐라도 먹어야 해.", e:"You should have something before you leave."},
            {d:2, s:"교재1", k:"피자 먹으면서 영화 보자.", e:"Let’s have some pizza and watch a movie."},
            {d:2, s:"교재2", k:"수업 전에 뭐 좀 먹었어?", e:"Did you have anything before class?"},
            {d:2, s:"교재2", k:"그냥 바나나 하나 먹었어. 시간이 없었어.", e:"Just a banana. I was in a rush."},
            {d:2, s:"교재2", k:"점심은 주로 몇 시에 먹어요?", e:"What time do you usually have lunch?"},
            {d:2, s:"교재2", k:"그렇게 안 바쁘면 대략 12시쯤에요.", e:"Around noon, if I’m not too busy."},
            {d:2, s:"교재2", k:"오, 저도 그런데! 오늘 저랑 뭐 간단히 먹을래요?", e:"Oh, me too! Want to grab something together today?"},
            {d:2, s:"교재2", k:"좋지요! 한국 음식이라면 좋아요.", e:"Sure! I could go for some Korean food."},
            {d:2, s:"교재3", k:"어제 공원에서 널 봤어.", e:"I saw you at the park yesterday."},
            {d:2, s:"교재3", k:"어제 공원에서 널 지켜봤어.", e:"I watched you at the park yesterday."},
            {d:2, s:"교재3", k:"술집에 있는 TV에서 경기가 나오던데, 집중해서 보지는 않았어.", e:"I saw the game on the TV at the bar, but I didn’t watch it."},
            {d:2, s:"교재3", k:"그 영화를 대형 화면으로 봤어.", e:"I saw the movie on the big screen."},
            {d:2, s:"교재3", k:"어젯밤에 넷플릭스 다큐멘터리를 보고 있는데 엄마한테 전화가 왔다.", e:"My mom called while I was watching a Netflix documentary last night."},
            {d:3, s:"대표", k:"어제 아침에는 별일이 다 있었어요.", e:"I had a weird morning yesterday."},
            {d:3, s:"교재1", k:"우리는 지난주 일본에서 정말 좋은 시간을 보냈어요.", e:"We had a great time in Japan last week."},
            {d:3, s:"교재1", k:"나는 힘든 유년 시절을 보냈다.", e:"I had a rough childhood."},
            {d:3, s:"교재1", k:"제가 접속 상태가 안 좋은 것 같습니다.", e:"I think I have a bad connection."},
            {d:3, s:"교재1", k:"저희는 만나면 늘 대화가 즐겁습니다.", e:"We always have such good conversations."},
            {d:3, s:"교재1", k:"그는 비행기를 오래 타서, 잠을 좀 더 자야 해요.", e:"He had a long flight, so he needs some extra sleep."},
            {d:3, s:"교재2", k:"여전히 예술 학교에 진학할 계획이니?", e:"Are you still planning to go to art school?"},
            {d:3, s:"교재2", k:"잘 모르겠어. 전공 때문에 엄마랑 사이가 틀어졌거든.", e:"I’m not sure. My mom and I had a falling-out over my major."},
            {d:3, s:"교재2", k:"안녕, 최근에 잘 안 보이더라.", e:"Hey, I haven’t seen you around lately."},
            {d:3, s:"교재2", k:"응... 한 주 동안 마음이 좀 힘들었거든.", e:"I know... I had a rough week, emotionally."},
            {d:3, s:"교재2", k:"무슨 일인지 이야기해 줄래?", e:"Want to talk about it?"},
            {d:3, s:"교재2", k:"음, 다음에. 우선 머리 좀 식힐 시간이 필요해서.", e:"Maybe later. I just need some time to clear my head."},
            {d:3, s:"교재3", k:"내가 어렸을 때, 우리 이웃이 나무에서 떨어졌어요.", e:"My neighbor fell out of a tree when I was young."},
            {d:3, s:"교재3", k:"아무래도 버스에서 내리다 핸드폰이 주머니에서 빠진 것 같아.", e:"I think my phone fell out of my pocket while I was getting off the bus."},
            {d:3, s:"교재3", k:"그 두 사람 크리스마스 이후로 사이가 완전히 틀어졌대.", e:"Those two had a major falling-out after Christmas."},
            {d:3, s:"교재3", k:"제 동업자와 저는 돈 문제로 사이가 멀어졌어요.", e:"My business partner and I had a falling-out over money."},
            {d:4, s:"대표", k:"삼성이 오늘 신제품 출시 행사를 열어요.", e:"Samsung is having a new-product launch today."},
            {d:4, s:"교재1", k:"제가 이번 주말에 집들이를 합니다.", e:"I’m having a housewarming party this weekend."},
            {d:4, s:"교재1", k:"백화점들이 이번 주에 일제히 세일을 한다.", e:"The department stores are all having sales this week."},
            {d:4, s:"교재1", k:"우리는 6월에 코엑스에서 채용 박람회를 열었다.", e:"We had a job fair at COEX in June."},
            {d:4, s:"교재1", k:"이번 주말에 모임이 있으니 오고 싶으면 오렴.", e:"We’re having a get-together this weekend if you want to join."},
            {d:4, s:"교재1", k:"그 제과점에서 오늘 원 플러스 원 행사를 한다.", e:"The bakery has a buy-one-get-one deal today."},
            {d:4, s:"교재2", k:"여행용 가방을 새로 사야 해.", e:"I need to buy some new luggage."},
            {d:4, s:"교재2", k:"아, 코스트코에서 지금 샘소나이트 여행 가방 세일 중인데.", e:"Oh, Costco is having a sale on Samsonite luggage right now."},
            {d:4, s:"교재2", k:"준호 씨, 브라질 생활 어때요?", e:"How is life in Brazil treating you, Junho?"},
            {d:4, s:"교재2", k:"쉽지는 않지만 나아지고 있습니다.", e:"It’s not easy, but it’s getting better."},
            {d:4, s:"교재2", k:"지금쯤이면 친구도 사귀었겠어요?", e:"Do you have any friends yet?"},
            {d:4, s:"교재2", k:"아파트 단지에 한국인 가정이 매주 바비큐 파티를 해요.", e:"A couple of Korean families live in my building and we have barbeques every Friday."},
            {d:4, s:"교재3", k:"퇴근은 한 거지?", e:"Did you leave work yet?"},
            {d:4, s:"교재3", k:"엄마한테 전화한 거지?", e:"Did you call your mom yet?"},
            {d:4, s:"교재3", k:"저녁은 먹은 거지?", e:"Did you eat dinner yet?"},
            {d:4, s:"교재3", k:"집안일은 다 한 거지?", e:"Did you finish your chores yet?"},
            {d:4, s:"교재3", k:"당신 이미 집 나선 거지?", e:"Did you leave yet?"},
            {d:5, s:"대표", k:"오후에는 집안일을 좀 해야 해요.", e:"I have some chores to do in the afternoon."},
            {d:5, s:"교재1", k:"할 일이 산더미같이 쌓여 있다.", e:"I have tons of work to do."},
            {d:5, s:"교재1", k:"답장해야 할 이메일이 몇 개 있다.", e:"I have a few emails to reply to."},
            {d:5, s:"교재1", k:"오늘 오후에 몇 가지 볼일이 좀 있다.", e:"I have some errands to run this afternoon."},
            {d:5, s:"교재1", k:"금요일까지 프로젝트 하나를 마무리해야 한다.", e:"We have a project to finish by Friday."},
            {d:5, s:"교재1", k:"크리스마스 쇼핑을 좀 해야 한다.", e:"I have some Christmas shopping to do."},
            {d:5, s:"교재2", k:"안녕하세요, 제리. 우리 방금 프로젝트 진행 상황에 대해 이야기하고 있었어요.", e:"Good morning, Jerry. We were just talking about the project updates."},
            {d:5, s:"교재2", k:"네, 늦어서 죄송해요. 뭐 좀 처리할 게 있었어요.", e:"Yes, sorry I’m late. I had something to take care of."},
            {d:5, s:"교재2", k:"오늘 바빠? 점심 먹자.", e:"Are you busy today? Let’s have lunch."},
            {d:5, s:"교재2", k:"안 돼. 우체국 가는 길이야.", e:"I can’t. I’m on my way to the post office."},
            {d:5, s:"교재2", k:"중고나라에서 바지 다섯 개를 팔아서, 택배 부칠 게 많아.", e:"I just sold five pairs of pants, so I have a lot of packages to ship."},
            {d:5, s:"교재3", k:"회사에서 모든 경비를 다 대 줬어요.", e:"The company took care of everything."},
            {d:6, s:"대표", k:"교토에 있는 기온 거리는 분위기가 참 매력적이에요.", e:"The Gion district in Kyoto has a charming vibe."},
            {d:6, s:"교재1", k:"이 컵은 손잡이가 독특하다.", e:"This cup has a unique handle."},
            {d:6, s:"교재1", k:"내가 새로 산 차는 시트가 가죽이다.", e:"My new car has leather seats."},
            {d:6, s:"교재1", k:"이 바지는 주머니가 없다.", e:"These pants don’t have pockets."},
            {d:6, s:"교재1", k:"새 모델은 디자인이 아주 잘 빠졌다.", e:"The new model has a sleek design."},
            {d:6, s:"교재1", k:"그 사람은 성격이 참 좋아요", e:"He has a great personality."},
            {d:6, s:"교재2", k:"난 북촌에서 하루 종일 시간 보낼 수 있어.", e:"I could spend all day in Bukchon."},
            {d:6, s:"교재2", k:"무슨 말인지 알지. 북촌이 나름의 운치가 있잖아.", e:"I know what you mean. Bukchon has a lot of character."},
            {d:6, s:"교재2", k:"그분 정치인치고는 재미있는 배경을 가지고 있어.", e:"He has an interesting background for a politician."},
            {d:6, s:"교재2", k:"맞아. 배우이자 프로 레슬링 선수였으니.", e:"He does. He used to be an actor and a pro-wrestler."},
            {d:6, s:"교재2", k:"그래서 카메라 앞에서 그렇게 편안한 거야.", e:"That explains why he’s so comfortable in front of the camera."},
            {d:6, s:"교재2", k:"그런 듯. 근데 세금 좀 낮춰 줬으면∙∙∙.", e:"I guess so, but I just hope he lowers taxes…"},
            {d:6, s:"교재3", k:"당신은 목소리가 좋군요.", e:"You have a good voice."},
            {d:6, s:"교재3", k:"남산 하얏트는 야경이 끝내주지.", e:"The Namsan Hyatt has a great night view."},
            {d:6, s:"교재3", k:"당신의 논리에는 모순이 있어요.", e:"Your logic has some flaws."},
            {d:6, s:"교재3", k:"홍삼은 건강에 정말 좋다.", e:"Red ginseng has a lot of health benefits."},
            {d:6, s:"교재3", k:"어느 아파트가 더 마음에 들어?", e:"Which apartment do you like better?"},
            {d:6, s:"교재3", k:"첫 번째 곳은 강 근처이긴 하지만 주차가 안 되니….", e:"The first place is near the river, but there’s no parking…"},
            {d:6, s:"교재3", k:"그래, 두 번째 곳은 주차는 되지만 붐비는 거리 바로 옆이고.", e:"Yeah, and the second place has parking, but it’s next to a daily street."},
            {d:6, s:"교재3", k:"두 곳 다 장단점이 있네. 일단은 계속 알아보는 게 좋겠어.", e:"They both have pros and cons. Maybe we should keep looking, for now."},
            {d:7, s:"대표", k:"우리를 저녁 식사에 불러 줘서 고마워요.", e:"Thank you for having us over for dinner."},
            {d:7, s:"교재1", k:"나는 간단한 메모를 위해 항상 펜을 곁에 둔다.", e:"I always have a pen nearby to take quick notes."},
            {d:7, s:"교재1", k:"생일 저녁 식사에 누구를 부를 거니?", e:"Who are you having over for your birthday dinner?"},
            {d:7, s:"교재1", k:"다시 보니 너무 좋구나. 얼마나 보고 싶던지.", e:"It’s great to have you back. We missed you a lot."},
            {d:7, s:"교재1", k:"너 지금 수중에 현금 있니?", e:"Do you have any cash on you?"},
            {d:7, s:"교재1", k:"나는 차 안에 있을 때 라디오를 켜 두는 편이다.", e:"I like to have the radio on when I’m in the car."},
            {d:7, s:"교재2", k:"빌리, 할아버지 좀 혼자 있게 해 드려. 바쁘셔.", e:"Hey, Billy, leave Grandpa alone. He’s busy."},
            {d:7, s:"교재2", k:"빌리, 엄마 말 듣지 마. 네가 곁에 있으니 좋기만 한걸.", e:"Don’t listen to her, Billy. I like having you around."},
            {d:7, s:"교재2", k:"타라, 다음 주 금요일 내 생일 저녁 식사에 올래요?", e:"Tara, would you like to come to my birthday dinner next Friday?"},
            {d:7, s:"교재2", k:"물론이죠. 갈게요. 또 누가 오나요?", e:"Of course. I’ll be there. Who else are you having over?"},
            {d:7, s:"교재2", k:"고등학교 친구 몇 명이 오긴 하는데, 주로 회사 사람들이 와요.", e:"A couple friends from high school, but mostly work people."},
            {d:7, s:"교재2", k:"좋네요. 새로운 친구를 사귈 수도 있겠네요. 초대해 줘서 고마워요.", e:"Cool. Maybe I can make some new friends. Thanks for having me."},
            {d:7, s:"교재3", k:"그녀를 두어 번 만났다.", e:"I’ve met her a couple (of) times."},
            {d:7, s:"교재3", k:"여름 방학이 시작되고 이틀이 지나자 벌써 학교가 좀 그리웠다.", e:"A couple of days into summer vacation, I already missed school a little."},
            {d:7, s:"교재3", k:"이거 마무리하려면 몇 분 더 필요해.", e:"I need a few minutes to finish this."},
            {d:7, s:"교재3", k:"친구 몇 명을 초대했다.", e:"I invited a few friends over."},
            {d:7, s:"교재3", k:"아들이 시험에서 실수를 여러 개 했어요.", e:"My son made several mistakes on the test."},
            {d:7, s:"교재3", k:"잠에서 깬 뒤, 가끔은 몇 분 동안 그냥 누워 있다가 결국 일어나요.", e:"After I wake up, I sometimes just lie there for several minutes before finally getting up."},
            {d:8, s:"대표", k:"다니엘에게 태워 달라고 할 수도 있잖아요.", e:"You could have Daniel give you a ride."},
            {d:8, s:"교재1", k:"남편한테 역 근처에서 만나라고 할게요.", e:"I’ll have my husband meet you near the station."},
            {d:8, s:"교재1", k:"제프한테 운동 조언을 좀 부탁하면 어떨까?", e:"Maybe you could have Jeff give you some exercise tips."},
            {d:8, s:"교재1", k:"저희 비서한테 다시 전화드리라고 해 둘게요.", e:"I will have my secretary get back to you."},
            {d:8, s:"교재1", k:"그분한테 내 사무실로 들르라고 해요. 이력서 검토해 볼게요.", e:"Have him stop by my office and I’ll take a look at his résumé."},
            {d:8, s:"교재1", k:"앤드루한테 초안 감수 부탁할까 싶어요.", e:"I was thinking of having Andrew proofread my draft."},
            {d:8, s:"교재2", k:"얼마 전에 이 컴퓨터 샀는데 하루에도 열 번씩 먹통이 되네.", e:"I just bought this computer and it crashes like ten times a day."},
            {d:8, s:"교재2", k:"니콜라스한테 한번 봐 달라고 해 봐. 그 친구 컴퓨터 잘 다루잖아.", e:"You should have Nicholas check it out. He’s good with computers."},
            {d:8, s:"교재2", k:"김앤김 법률 사무소입니다. 어디로 연결해 드릴까요?", e:"Kim & Kim Law Office; how may I direct your call?"},
            {d:8, s:"교재2", k:"안녕하세요, 김유나 변호사님과 통화 가능할까요?", e:"Hello, may I speak with Yuna Kim?"},
            {d:8, s:"교재2", k:"지금은 의뢰인과 함께 계셔서요. 메시지 전해 드릴까요?", e:"She is currently with a client right now. May I take a message?"},
            {d:8, s:"교재2", k:"네, 변호사님의 의뢰인 케빈 리입니다. 저에게 전화 부탁드린다고 전해 주시겠어요?", e:"Yes, I’m a client of hers—Kevin Lee. Could you have her call me back, please?"},
            {d:8, s:"교재3", k:"신분증 좀 볼 수 있을까요?", e:"May I see your ID?"},
            {d:8, s:"교재3", k:"전화 주신 분은 누구신가요?", e:"May I ask who’s calling?"},
            {d:8, s:"교재3", k:"죄송한데 먼저 일어나 봐도 될까요?", e:"May I go now?"},
            {d:8, s:"교재3", k:"들어가도 될까요?", e:"May I come in?"},
            {d:8, s:"교재3", k:"로비에서 기다려 주시겠어요?", e:"May I ask you to take a seat in the lobby, please?"},
            {d:9, s:"대표", k:"남자 친구가 콘서트 표를 구했어요.", e:"My boyfriend got a ticket to the concert."},
            {d:9, s:"교재1", k:"지난주에 이 핸드폰 샀어.", e:"I got this phone last week."},
            {d:9, s:"교재1", k:"이 신발 이탈리아에서 구했어.", e:"I got these shoes from Italy."},
            {d:9, s:"교재1", k:"이 그림 벼룩시장에서 샀어.", e:"I got this painting at a flea market."},
            {d:9, s:"교재1", k:"이 가죽 재킷은 어디서 산 거야?", e:"Where did you get this leather jacket?"},
            {d:9, s:"교재1", k:"이 주변에서 커피 살 수 있는 곳이 어딘가요?", e:"Where can I get a coffee around here?"},
            {d:9, s:"교재2", k:"이 명품 가방은 어떻게 구한 거야?", e:"How did you get this designer bag?"},
            {d:9, s:"교재2", k:"온라인에서 샀어. 소매가보다 두 배 비싸지만, 그 값은 하더라.", e:"I bought it online. It was twice as much as the retail price, but it was worth it."},
            {d:9, s:"교재2", k:"코비 8 신발 어디서 구할 수 있는지 아니?", e:"Do you know where I can get a pair of Kobe 8’s?"},
            {d:9, s:"교재2", k:"내가 아는 곳에 데려갈게. 어쩌면 거기 있을지도 몰라.", e:"I can take you to a place I know. They might have some."},
            {d:9, s:"교재2", k:"응, 좋아. 온라인에서 사고 싶지는 않거든.", e:"Yeah, that’d be good. I don’t want to buy them online."},
            {d:9, s:"교재2", k:"응, 요즘 짝퉁 스니커즈 파는 사람이 많긴 해.", e:"Yeah, there are a lot of knock-off sneaker dealers these days."},
            {d:9, s:"교재3", k:"이 속편은 원작의 절반에도 못 미치네요.", e:"This sequel isn’t even half as good as the original."},
            {d:9, s:"교재3", k:"신형 하이브리드 모델은 이전 모델 대비 탄소 배출량이 세 배나 적다.", e:"The new hybrid model has three times less carbon emissions than the previous version."},
            {d:9, s:"교재3", k:"성수동 집세(월세)가 우리 동네보다 세 배나 더 비싸다.", e:"Rent in Seongsu is three times higher than in my area."},
            {d:9, s:"교재3", k:"이 샌드위치가 빅맥에 비해 칼로리가 두 배 이상 높다.", e:"This sandwich has more than double the calories of a Big Mac."},
            {d:9, s:"교재3", k:"저는 지금 하는 일도 좋습니다. 급여는 예전 수준의 반도 안 되지만요.", e:"I still like my current job, even though they don’t even pay half what I used to earn."},
            {d:10, s:"대표", k:"거기 자리 잡으려면 엄청 미리 예약해야 해요.", e:"You have to book way in advance to get a table there."},
            {d:10, s:"교재1", k:"드디어 서울 마라톤 대회 참가 자격을 얻었다.", e:"I finally managed to get a spot in the Seoul marathon."},
            {d:10, s:"교재1", k:"이번 여름에 샌프란시스코에서 인턴을 하고 싶습니다.", e:"I’m hoping to get an internship in San Francisco this summer."},
            {d:10, s:"교재1", k:"장학금을 못 받으면 이번 학기를 못 다닐 것 같아.", e:"If I don’t get a scholarship, I won’t be able to go this semester."},
            {d:10, s:"교재1", k:"우리는 강남에는 집 못 얻을 듯해.", e:"I don’t think we can get a place in Gangnam."},
            {d:10, s:"교재1", k:"무대 가까운 자리를 못 구했어.", e:"We couldn’t get a spot near the stage."},
            {d:10, s:"교재2", k:"그 로펌에서 면접 보게 된 거지?", e:"Did you get an interview at that law firm yet?"},
            {d:10, s:"교재2", k:"아니, 아직 기다리는 중이야.", e:"Not yet, I’m still waiting"},
            {d:10, s:"교재2", k:"내 생일에 그 식당 가는 거 너무 기대돼.", e:"I’m so excited to go to that restaurant for my birthday."},
            {d:10, s:"교재2", k:"사실 예약 못 했어. 앞으로 3개월 동안 예약이 꽉 찼더라고.", e:"Actually, I couldn’t get a reservation. They’re booked for the next three months."},
            {d:10, s:"교재2", k:"정말? 너무 아쉽다.", e:"Seriously? That’s a bummer."},
            {d:10, s:"교재2", k:"응, 그래도 가끔 취소하는 사람도 있으니까. 우리 그냥 매일 계속 확인해 보자.", e:"Yeah, but sometimes people cancel. We just have to keep checking every day."},
            {d:10, s:"교재3", k:"테일러 스위프트 콘서트 표 구했어?", e:"Did you get tickets to the Taylor Swift concert?"},
            {d:10, s:"교재3", k:"아니, 몇 분 만에 매진되었어. 너무 속상했어!", e:"No, they sold out in minutes. I was so bummed out!"},
            {d:10, s:"교재3", k:"무지 실망스러웠다.", e:"It was a big disappointment."},
            {d:11, s:"대표", k:"퇴근하고 한잔해요.", e:"Let’s get a drink after work."},
            {d:11, s:"교재1", k:"혹시 많이 안 바쁘시면, 언제 술이나 한잔해도 좋고요.", e:"If you’re not too busy, maybe we could get a drink sometime."},
            {d:11, s:"교재1", k:"오늘 퇴근하고 폴 만나서 맥주 한잔하려고.", e:"I’m going to get a beer with Paul after work today."},
            {d:11, s:"교재1", k:"회의 끝나고 점심 하자. 내가 살게.", e:"After the meeting, let’s get lunch. My treat."},
            {d:11, s:"교재1", k:"오늘 밤에 저녁 먹으면서 와인 한 병 하자.", e:"Let’s get a bottle of wine with dinner tonight."},
            {d:11, s:"교재1", k:"내일 바빠요? 커피 한잔해요.", e:"Are you busy tomorrow? Let’s get coffee."},
            {d:11, s:"교재2", k:"늦었긴 한데 그래도 피곤하지는 않아.", e:"It’s late, but I don’t feel tired."},
            {d:11, s:"교재2", k:"나가서 한잔하자. 정말 좋은 데 알거든.", e:"Let’s go get a drink. I know a great place."},
            {d:11, s:"교재2", k:"너랑 니콜라스 못 본 지도 한참 됐네.", e:"I haven’t seen you and Nicholas for a while."},
            {d:11, s:"교재2", k:"맞아, 한참 됐어.", e:"I know, it has been a long time."},
            {d:11, s:"교재2", k:"4월에 언제 점심이나 저녁 할까?", e:"Why don’t we get lunch or dinner sometime in April?"},
            {d:11, s:"교재2", k:"너무 좋지. 그때쯤이면 날씨도 좋을 거야.", e:"That sounds great. The weather should be nice by then."},
            {d:11, s:"교재3", k:"뷔페에서 너무 많이 먹었어요.", e:"I ate too much at the buffet."},
            {d:11, s:"교재3", k:"그 사람 지금 사과를 먹고 있어요.", e:"He is eating an apple right now."},
            {d:11, s:"교재3", k:"돼지고기는 안 먹어요.", e:"I don’t eat pork."},
            {d:11, s:"교재3", k:"우리는 오늘 점심으로 파스타 먹었어요.", e:"We had pasta for lunch today."},
            {d:11, s:"교재3", k:"헬스장 가기 전에 주로 시리얼을 먹어요.", e:"I usually have cereal before the gym."},
            {d:11, s:"교재3", k:"오늘 밥 될 만한 걸 아무것도 못 먹었어요.", e:"I haven’t had anything substantial today."},
            {d:11, s:"교재3", k:"우리는 같은 동네에 살아서, 가끔씩 만나서 커피 마십니다.", e:"We live in the same area, so we get coffee every once in a while."},
            {d:11, s:"교재3", k:"우린 맨날 말로는 언제 한잔하자고는 하는데, 한 번도 한 적이 없네요.", e:"We always say we’ll get a drink sometime, but it never happens."},
            {d:12, s:"대표", k:"아메리카노 라지 사이즈로 하나 주세요.", e:"Can I get a large Americano?"},
            {d:12, s:"교재1", k:"레몬차 주시겠어요?", e:"Can I get a lemon tea, please?"},
            {d:12, s:"교재1", k:"커피 테이크아웃으로 한 잔 부탁해요.", e:"Can I get a coffee to go, please?"},
            {d:12, s:"교재1", k:"햄버거에 치즈 한 장 더 얹어 주시겠어요?", e:"Can I get extra cheese on my burger?"},
            {d:12, s:"교재1", k:"물티슈 한 장 주시겠어요?", e:"Can I get a wet wipe, please?"},
            {d:12, s:"교재1", k:"식사에 포함된 감자튀김 대신 샐러드로 주실 수 있을까요?", e:"Can we get salad instead of fries with our dinner?"},
            {d:12, s:"교재2", k:"(호텔 프런트에 전화로) 수건 좀 더 주시겠어요? 502호예요.", e:"Can I get some more towels? I’m in room 502."},
            {d:12, s:"교재2", k:"물론입니다. 문 앞에 둘게요.", e:"Sure, we’ll leave them in front of your door."},
            {d:12, s:"교재2", k:"뭐 드릴까요?", e:"What can I get you?"},
            {d:12, s:"교재2", k:"네, 소이라테 주시겠어요?", e:"Yeah, can I get a soy latte?"},
            {d:12, s:"교재2", k:"죄송한데, 지금 두유가 다 떨어졌어요.", e:"Oh, I’m sorry, we are out of soymilk."},
            {d:12, s:"교재2", k:"아, 그럼 그냥 홍차로 할게요.", e:"Oh, then I’ll just have a black tea."},
            {d:12, s:"교재3", k:"저 사람들이 먹는 거랑 같은 것으로 주시겠어요?", e:"Can I get the same thing they’re having?"},
            {d:12, s:"교재3", k:"이 20달러 지폐를 잔돈으로 바꿔 주실 수 있을까요?", e:"Can I get change for this $20, please?"},
            {d:12, s:"교재3", k:"이 점에 대한 당신의 생각을 말해 주시겠어요?", e:"Can I get your thoughts on this?"},
            {d:13, s:"대표", k:"(여행 갔다가) 어젯밤에 돌아왔어요.", e:"We just got back last night."},
            {d:13, s:"교재1", k:"나 방금 도착했어.", e:"I just got here."},
            {d:13, s:"교재1", k:"언제 도착한 거야?", e:"When did you get here?"},
            {d:13, s:"교재1", k:"역에 도착하면 문자 줘.", e:"Text me when you get to the station."},
            {d:13, s:"교재1", k:"회의에 늦지 않게 간 거야?", e:"Did you get to the meeting on time?"},
            {d:13, s:"교재1", k:"이 업계에서 성공하고 싶으면 골프를 배워야 해요.", e:"If you want to get anywhere in this business, you need to learn how to golf."},
            {d:13, s:"교재2", k:"집에는 어떻게 가세요? 태워 드릴까요?", e:"How are you getting home? Do you need a ride?"},
            {d:13, s:"교재2", k:"지하철을 타려고요. 역까지 태워 주시면 너무 좋죠, 고마워요.", e:"I’m taking the subway. A ride to the station would be great, thanks."},
            {d:13, s:"교재2", k:"(팀장이 팀원에게) 이 회사에서의 목표가 뭔가요?", e:"What are your goals at this company?"},
            {d:13, s:"교재2", k:"음, 팀장님 위치까지 올라가 보고 싶어요. 해 주실 조언이 있을까요?", e:"Well, I’d like to get to where you are. Do you have any advice?"},
            {d:13, s:"교재2", k:"내 자리까지 오려면, 다른 부서에서 어떤 일을 하는지도 잘 알아야 해요.", e:"To get where I am, you’ll need to know what other departments do."},
            {d:13, s:"교재2", k:"알겠습니다. 그럼 다른 부서 사람들과 점심 식사도 하려고 해야겠군요.", e:"I see. So I should try to have lunch with people from other departments."},
            {d:13, s:"교재3", k:"늦어서 미안. 오래 기다렸어?", e:"Sorry I’m late. Were you waiting long?"},
            {d:13, s:"교재3", k:"아니, 아니. 도착한 지 몇 분 안 됐어.", e:"No, no. I just got here a couple minutes ago."},
            {d:13, s:"교재3", k:"어, 다니엘, 네가 간 줄 알았어.", e:"Hey, Daniel, I thought you left."},
            {d:13, s:"교재3", k:"그랬는데. 우산 가지러 다시 왔어.", e:"I did. I just came back to get my umbrella."},
            {d:14, s:"대표", k:"그녀가 왜 그렇게 화가 났는지 이해가 안 돼요.", e:"I don’t get why she’s so upset."},
            {d:14, s:"교재1", k:"이 수학 문제 이해가 안 돼요.", e:"I don’t get this math problem."},
            {d:14, s:"교재1", k:"어떤 부분이 이해가 안 되는데?", e:"What don’t you get?"},
            {d:14, s:"교재1", k:"이해가 안 됩니다. 다시 한번 설명해 주시겠어요?", e:"I don’t get it. Can you explain it again?"},
            {d:14, s:"교재1", k:"무슨 말인지 완전히 알겠습니다.", e:"I totally get what you’re saying."},
            {d:14, s:"교재1", k:"시간이 좀 걸렸지만, 결국 개념을 이해했다.", e:"It took me a while, but I finally got the concept."},
            {d:14, s:"교재1", k:"그는 누구보다 나를 잘 안다.", e:"He gets me like no one else does."},
            {d:14, s:"교재2", k:"사라 생일이야. 꽃을 사 줘야 해.", e:"It’s Sarah’s birthday. I need to get her flowers."},
            {d:14, s:"교재2", k:"잠깐만, 이해가 안 돼. 너희들 헤어졌잖아.", e:"Wait, I don’t get it. You guys broke up."},
            {d:14, s:"교재2", k:"또 말도 안 되는 부동산 정책을?", e:"Another bad real estate policy?"},
            {d:14, s:"교재2", k:"응, 이 후보는 상황 파악을 못하네.", e:"Yeah, this candidate just doesn’t get it."},
            {d:14, s:"교재2", k:"정말! 맨날 똑같은 정책만 내.", e:"Seriously! They always do the same thing."},
            {d:14, s:"교재2", k:"맞아. 도대체 무슨 생각인지 모르겠어.", e:"Right. I don’t get their logic."},
            {d:14, s:"교재3", k:"날씨가 너무 이상해.", e:"This weather is so weird."},
            {d:14, s:"교재3", k:"맞아. 하루는 따뜻하고 해가 났다가, 다음 날은 춥고 비 오고.", e:"Seriously. One day it’s warm and sunny, and the next day it’s cold and rainy."},
            {d:14, s:"교재3", k:"나 미국으로 가. 거기에 직장 구했거든.", e:"I’m moving to the U.S. I got a job there."},
            {d:14, s:"교재3", k:"정말? 와. 축하해.", e:"Seriously? Wow. Congratulations."},
            {d:15, s:"대표", k:"제 영어 실력이 점점 녹슬고 있는 것 같아요.", e:"I feel like my English is getting rusty."},
            {d:15, s:"교재1", k:"요즘 날씨가 더 추워지고 있다.", e:"It’s getting colder these days."},
            {d:15, s:"교재1", k:"나는 중요한 시험 전에는 항상 긴장한다.", e:"I always get nervous before a big exam."},
            {d:15, s:"교재1", k:"한동안 목을 좀 쉬게 했더니 나아졌다.", e:"My throat got better after resting my voice for a while."},
            {d:15, s:"교재1", k:"두 남자가 싸우기 시작하면서 파티가 난장판이 되었다.", e:"The party got wild after two guys started fighting."},
            {d:15, s:"교재1", k:"그 넷플릭스 드라마 촬영 이후로 그 지역이 정말 인기가 많아졌다.", e:"The area got really popular after that Netflix show was filmed there."},
            {d:15, s:"교재2", k:"어젯밤에 상사랑 저녁 식사는 어땠어요?", e:"How was dinner with your boss last night?"},
            {d:15, s:"교재2", k:"그가 정치 이야기를 시작하는 바람에 어색해졌어요.", e:"It got awkward when he started talking about politics."},
            {d:15, s:"교재2", k:"(부동산 중개인에게) 이 아파트 정말 마음에 듭니다.", e:"I really like this apartment."},
            {d:15, s:"교재2", k:"좋습니다. 그런데 근처에 술집이 많아서 밤에는 좀 시끄러울 수 있다는 점은 염두에 두셔야 해요.", e:"That’s good, but you should keep in mind that it gets a bit noisy at night with all the bars nearby."},
            {d:15, s:"교재2", k:"아, 알려 주셔서 감사합니다. 그렇다면, 계속 찾아보죠.", e:"Oh, I’m glad you mentioned that. In that case, let’s keep looking."},
            {d:15, s:"교재2", k:"네, 좀 더 조용한 곳을 보여 드릴게요.", e:"OK, I have some quieter places to show you."},
            {d:15, s:"교재3", k:"나는 대체로 약속 시간에 늦는다. 그 때문에 자주 난처한 상황에 처한다.", e:"More often than not, I’m late for my appointments. That often puts me in awkward situations."},
            {d:15, s:"교재3", k:"제가 볼 때 〈이코노미스트〉 기사를 더 읽는 건 좋지 않습니다. 당신의 영어를 어색하게 만들 뿐입니다.", e:"I don’t think you should read any more articles from the Economist. All it does is make your English awkward."},
            {d:15, s:"교재3", k:"내 친구들 중에는 사회성이 부족한 사람이 많아요.", e:"Many of my friends are socially awkward."},
            {d:15, s:"교재3", k:"아는 사람이 없는 파티에 가면 늘 어색하고 불편해요.", e:"I always feel awkward at parties when I don’t know anyone."},

            /* Week 4 (Day 16-20) */
            {d:16, s:"대표", k:"커피를 안 마시면 두통이 생겨요.", e:"I get a headache if I don’t drink coffee."},
            {d:16, s:"교재1", k:"열대 지역으로 여행 가서 말라리아에 걸리는 사람들이 있다.", e:"Some people get malaria when they travel to tropical countries."},
            {d:16, s:"교재1", k:"요즘 심장병에 걸리는 젊은 사람들이 점점 늘고 있다.", e:"More and more young people are getting heart disease these days."},
            {d:16, s:"교재1", k:"설탕 안 줄이면 당뇨병에 걸릴 거야.", e:"If you don’t cut down on sugar, you’re going to get diabetes."},
            {d:16, s:"교재1", k:"내가 중학교에 다닐 때 엄마가 암에 걸리셨다.", e:"My mom got cancer when I was in middle school."},
            {d:16, s:"교재1", k:"나이 들어서 치매에 안 걸리려고 책을 많이 읽으려고 한다.", e:"I try to read a lot so I won’t get dementia when I’m older."},
            {d:16, s:"교재2", k:"내일 회의가 취소됐다고 하더라.", e:"I heard tomorrow’s meeting got cancelled."},
            {d:16, s:"교재2", k:"응, 빌이 폐렴에 걸려서 병원에 가야 했거든.", e:"Yeah, Bill got pneumonia and he had to go to the hospital."},
            {d:16, s:"교재2", k:"저기, 어제 굴 먹고 괜찮아?", e:"Hey, are you feeling OK after the oysters yesterday?"},
            {d:16, s:"교재2", k:"응, 나는 괜찮아. 왜?", e:"Yeah, I feel fine. Why?"},
            {d:16, s:"교재2", k:"식중독 걸린 것 같아. 오늘 속이 너무 안 좋아.", e:"I think I got food poisoning. My stomach is messed up today."},
            {d:16, s:"교재2", k:"저런, 어떡해. 화장실 근처에 있어야겠다.", e:"Oh, no. Sorry to hear that. Stay close to the toilet."},
            {d:16, s:"교재3", k:"퇴근하고 은행에 들러야 해.", e:"I need to go to the bank after work."},
            {d:16, s:"교재3", k:"치과에 예약이 있어.", e:"I have an appointment at the dentist."},
            {d:16, s:"교재3", k:"아직 사무실에 있어.", e:"I’m still in the office."},
            {d:17, s:"대표", k:"장 본 것을 집에 가져가야 해요.", e:"I need to get these groceries home."},
            {d:17, s:"교재1", k:"여권이 만료되기 직전에 서류를 대사관에 보냈어요.", e:"I got my documents to the embassy right before my passport expired."},
            {d:17, s:"교재1", k:"이 양식을 오늘 인사과로 보내야 해요.", e:"I need to get these forms to the HR department today."},
            {d:17, s:"교재1", k:"이 소파 어떻게 위층으로 옮기지?", e:"How are we going to get this couch upstairs?"},
            {d:17, s:"교재1", k:"7달러에 플라자 호텔까지 좀 가 주실 수 있나요?", e:"Can you get me to the Plaza Hotel for $7?"},
            {d:17, s:"교재1", k:"그녀가(여자 친구가) 꽤 취했습니다. 집까지 안전하게 데려다주실 수 있을까요?", e:"She's quite drunk. Could you make sure to get her home safely?"},
            {d:17, s:"교재2", k:"커피 한잔할 시간 될까?", e:"Do you think we have time to get a coffee?"},
            {d:17, s:"교재2", k:"안 돼. 회의 시작하기 전에 사무실에 이 음식 가져가야 해.", e:"No way. We need to get this food to the office before the meeting starts."},
            {d:17, s:"교재2", k:"정신없는 아침이야! 늦잠 자 버렸어.", e:"What a crazy morning! I overslept."},
            {d:17, s:"교재2", k:"아, 난 늦잠 자 버리는 거 너무 싫은데. 애들은?", e:"Oh, I hate when I do that. What about the kids?"},
            {d:17, s:"교재2", k:"어찌어찌해서 애들은 학교에 늦지 않게 등교시켰어.", e:"I somehow managed to get the kids to school on time."},
            {d:17, s:"교재2", k:"와, 잠도 더 자고 아무도 안 늦고. 환상적이네.", e:"Wow, you got extra sleep and no one was late. Perfect."},
            {d:17, s:"교재3", k:"당분간 우리 부모님 댁으로 들어가서 사는 건 어때?", e:"Why don't we move in with my parents for a while?"},
            {d:17, s:"교재3", k:"절대 안 돼. 농담이라도 그런 말 하지 마.", e:"No way. Don't even joke about that."},
            {d:17, s:"교재3", k:"눈 온다!", e:"It’s snowing!"},
            {d:17, s:"교재3", k:"말도 안 돼!", e:"No way!"},
            {d:17, s:"교재3", k:"케빈이 날 찼어.", e:"Kevin dumped me."},
            {d:17, s:"교재3", k:"말도 안 돼! 진짜야?", e:"No way! Seriously?"},
            {d:18, s:"대표", k:"이 동네는 햇빛이 잘 안 들어요.", e:"This area doesn't get much sunshine."},
            {d:18, s:"교재1", k:"여름에는 관광객들이 이곳을 많이 찾아요.", e:"We get a lot of tourists in the summer."},
            {d:18, s:"교재1", k:"그녀는 밤낮 할 것 없이 이메일, 문자, 전화를 받습니다.", e:"She gets emails, texts, and phone calls all day and night."},
            {d:18, s:"교재1", k:"들어 보니까 샌디에이고는 겨울에도 눈이 거의 안 온다고 하더라.", e:"I heard San Diego hardly gets any snow, even in winter."},
            {d:18, s:"교재1", k:"요즘은 주말에도 찾는 사람이 많지 않아요.", e:"Even on the weekend we don't get much foot traffic these days."},
            {d:18, s:"교재1", k:"우리 아파트는 바다를 향하고 있어서, 거의 매일 시원한 바닷바람이 불어온다.", e:"My apartment faces the ocean, so we get a fresh sea breeze most days."},
            {d:18, s:"교재2", k:"뒤뜰에 허브를 키워 보고 싶었어요.", e:"I was hoping to grow some herbs in the backyard."},
            {d:18, s:"교재2", k:"참고로 말씀드리자면, 이 동네는 오후에 햇볕이 잘 안 들어요.", e:"Just so you know, this neighborhood doesn't get much sunlight in the afternoon."},
            {d:18, s:"교재2", k:"오늘 예약 있어?", e:"Got any appointments today?"},
            {d:18, s:"교재2", k:"오후에 하나밖에 없네. 요즘 경기가 좀 안 좋아.", e:"Just one in the afternoon. Business has been slow lately."},
            {d:18, s:"교재2", k:"그치만 그냥 오는 손님들도 있지 않아?", e:"But aren't there walk-ins?"},
            {d:18, s:"교재2", k:"없어. 발길 자체가 많지 않아. 요즘은 토요일조차도 말이야.", e:"Nope. We don't get much foot traffic, even on Saturdays now."},
            {d:18, s:"교재3", k:"꼭 물을 충분히 섭취하세요.", e:"Make sure you get enough water."},
            {d:18, s:"교재3", k:"의사 선생님이 뭐라고 하든?", e:"What did the doctor say?"},
            {d:18, s:"교재3", k:"늘 똑같지 뭐. 운동 좀 하고, 잠 충분히 자라고∙∙∙.", e:"Same thing she always says: get some exercise and plenty of sleep…"},
            {d:18, s:"교재3", k:"집중이 안 돼. 몇 시간째 화면만 보고 있어.", e:"I can't focus. I've been staring at my screen for hours"},
            {d:18, s:"교재3", k:"나가서 산책 좀 하면서 신선한 바람 좀 쐐.", e:"Go take a walk and get some fresh air."},
            {d:19, s:"대표", k:"당신의 자리를 우리 바로 옆 자리로 맡아 두었어요.", e:"We got you a spot next to us."},
            {d:19, s:"교재1", k:"강아지 생일이라 케이크를 사 줬어.", e:"I got my dog a cake for his birthday."},
            {d:19, s:"교재1", k:"커피 한 잔 드릴까요?", e:"Can I get you a cup of coffee?"},
            {d:19, s:"교재1", k:"내가 담요 가져다줄게요.", e:"I will get you a blanket."},
            {d:19, s:"교재1", k:"참치김밥 사 왔어. 괜찮았으면 좋겠다.", e:"I got you a tuna kimbap. I hope that's OK."},
            {d:19, s:"교재1", k:"가브리엘이 학장님에게서 추천서를 받아다 주었다.", e:"Gabrielle got me a recommendation letter from the dean."},
            {d:19, s:"교재2", k:"존 리 콘서트 표를 구해 왔다고? 어떻게?", e:"You got us tickets to the John Lee concert? How?"},
            {d:19, s:"교재2", k:"회사 동료가 못 가서, 나한테 표를 줬어.", e:"A friend at work can’t go, and she offered them to me."},
            {d:19, s:"교재2", k:"지난 주말 네 생일에 뭐 했어?", e:"What did you do for your birthday last weekend?"},
            {d:19, s:"교재2", k:"남편이랑 같이 콘래드 호텔에 가서 칵테일 마셨어.", e:"My husband and I had cocktails at the Conrad Hotel."},
            {d:19, s:"교재2", k:"재미있었겠다.", e:"That sounds fun."},
            {d:19, s:"교재2", k:"예약이 다 찼었는데, 직원이 강이 보이는 자리를 마련해 줬어.", e:"They were fully booked, but the server managed to get us a booth with a view of the river."},
            {d:19, s:"교재3", k:"올해 크리스마스 때는 유럽에 가 보는 게 어때?", e:"Why don't we go to Europe for Christmas this year?"},
            {d:19, s:"교재3", k:"아이들한테 어린이날 선물 뭐 해 줬어요?", e:"What did you get your kids for Children's Day?"},
            {d:19, s:"교재3", k:"이번 여름 휴가 계획 잡았어요?", e:"Do you have any anything planned for this summer vacation?"},
            {d:19, s:"교재3", k:"이번 추석 때는 뭐해?", e:"What are you doing for Chuseok this year?"},
            {d:19, s:"교재3", k:"아내 생일이 5월 5일이라, 우리는 보통 5월 중순쯤에 그 기념으로 근사한 오마카세 식당에 가곤 해요.", e:"We usually go to a nice omakase restaurant in mid-May for my wife's birthday, since she was born on May 5th."},
            {d:19, s:"교재3", k:"올해 내 생일날에 캐리비안 베이 가자.", e:"Let's go to Caribbean Bay on my birthday this year."},
            {d:19, s:"교재3", k:"어린이날에 수족관에 다녀왔습니다.", e:"We went to the aquarium on Children's Day."},
            {d:20, s:"대표", k:"어제 머리 염색했어요.", e:"I got my hair dyed yesterday."},
            {d:20, s:"교재1", k:"내 차의 앞 유리를 교체해야 한다.", e:"I need to get my windshield replaced on my car."},
            {d:20, s:"교재1", k:"내 컴퓨터를 고쳐야 한다.", e:"I have to get my computer fixed."},
            {d:20, s:"교재1", k:"이 서류에 사인을 받아야 한다.", e:"I need to get this document signed."},
            {d:20, s:"교재1", k:"다음 주에 사랑니를 뽑아야 한다.", e:"I need to get my wisdom teeth removed next week."},
            {d:20, s:"교재1", k:"회계사한테 맡겨서 세금 신고를 했다.", e:"I got my taxes done by an accountant."},
            {d:20, s:"교재2", k:"구매한 후에 여기서 바지 단 줄일 수 있나요?", e:"Can I get these pants hemmed here after I buy them?"},
            {d:20, s:"교재2", k:"물론이죠. 30분 정도밖에 안 걸릴 겁니다.", e:"Of course. It should only take about thirty minutes."},
            {d:20, s:"교재2", k:"제가 보기에는 리포트 정말 잘 쓰셨어요. 고칠 게 별로 없었어요.", e:"Your paper looks great to me. I didn’t need to edit much."},
            {d:20, s:"교재2", k:"정말요? 저는 아직 좀 더 다듬어야 할 것 같거든요.", e:"Really? I still feel like it's lacking some polish."},
            {d:20, s:"교재2", k:"제가 혹시 놓친 게 없는지 확인 차원에서 다른 분에게 교정 받는 것도 괜찮아요.", e:"You can get it proofread by someone else, just to be sure I didn't miss anything."},
            {d:20, s:"교재2", k:"아뇨, 그럴 필요까진 없을 것 같아요. 선생님이 잘 판단해 주셨을 거라 믿어요.", e:"Nah, I don't think that's necessary. I trust your judgment."},
            {d:20, s:"교재3", k:"내가 볼 땐 그게 더 나은 계획인 듯해.", e:"That sounds like a better plan to me."},
            {d:20, s:"교재3", k:"내가 볼 땐 흰색이 더 편할 듯해.", e:"To me, the white one seems more comfortable."},
            {d:20, s:"교재3", k:"그곳은 너한테 너무 완벽한 곳인 것 같아.", e:"That sounds like a perfect place for you."},
            {d:20, s:"교재3", k:"저에겐 재택근무는 불가능합니다.", e:"For me, working from home isn't an option."},

            /* Week 5 (Day 21-25) */
            {d:21, s:"대표", k:"어떻게 애들이 채소를 먹게 만들 수 있을까요?", e:"How can I get my kids to eat vegetables?"},
            {d:21, s:"교재1", k:"어떻게 남편이 담배 끊게 만든 거야?", e:"How did you get your husband to quit smoking?"},
            {d:21, s:"교재1", k:"아무리 제시간에 일어나려고 해도 잘 안 돼요.", e:"I can't get myself to wake up at a decent time."},
            {d:21, s:"교재1", k:"이 책상 옮기려고 해도 안 움직여요.", e:"I can't get this desk to move."},
            {d:21, s:"교재1", k:"이 기름때가 도무지 안 지워져요.", e:"I can't get this grease to come off."},
            {d:21, s:"교재1", k:"프린터 작동시키는 데 한참 걸렸어요.", e:"It took me forever to get the printer to work."},
            {d:21, s:"교재2", k:"(회사 동료에게) 이 많은 일을 어떻게 다 하죠?", e:"How am I going to finish all this work?"},
            {d:21, s:"교재2", k:"인턴 중 아무나 몇 명에게 대신 맡겨 봐요.", e:"Just get a few random interns to do it for you."},
            {d:21, s:"교재2", k:"그 사람이 내 차를 긁고는 아무 일 없었다는 것처럼 가 버렸지 뭐야.", e:"He scratched my car and just walked away like nothing happened."},
            {d:21, s:"교재2", k:"정말? 그러면 안 되지.", e:"Seriously? That’s not OK."},
            {d:21, s:"교재2", k:"맞아, 근데 괜히 일을 크게 만들고 싶지 않아서.", e:"I know, but I don’t want to make a big deal out of it."},
            {d:21, s:"교재2", k:"그래도 그 사람이 변상하게 만들어야지!", e:"Still, you should get him to pay for the damages!"},
            {d:21, s:"교재3", k:"그 가게는 아무 때나 문을 닫는다. (문 닫는 시간이 일정하지 않다.)", e:"They close at random hours."},
            {d:21, s:"교재3", k:"그 친구는 볼 때마다 다른 여자랑 있어.", e:"I always see him with random girls."},
            {d:21, s:"교재3", k:"지하철에서 낯선 사람이 소리를 지르고 있었다.", e:"Some random guy was yelling on the subway."},
            {d:21, s:"교재3", k:"사라가 지금 부동산 중개인 되었다는 거 알아?", e:"Did you know Sarah is a real estate agent now?"},
            {d:21, s:"교재3", k:"그 걸 그룹 멤버? 정말 뜬금없네.", e:"The girl from that K-pop group? That's random."},
            {d:22, s:"대표", k:"갈비찜 먹고 싶으면 내가 해 줄게요.", e:"I can make you steamed galbi if you want."},
            {d:22, s:"교재1", k:"아내가 이 목도리를 만들어 줬어요.", e:"My wife made me this scarf."},
            {d:22, s:"교재1", k:"남자 친구가 끈을 이용해서 팔찌를 만들어 줬어요.", e:"My boyfriend made me a bracelet from string."},
            {d:22, s:"교재1", k:"원하시면 차 한 잔 타 드릴게요.", e:"I can make you tea, if you’d like."},
            {d:22, s:"교재1", k:"우리 학교 선생님은 크리스마스 때 우리들에게 쿠키를 만들어 주시곤 했다.", e:"My school teacher would make us cookies for Christmas."},
            {d:22, s:"교재1", k:"뭐 먹을 것 좀 만들어 줄게.", e:"Let me make you something to eat."},
            {d:22, s:"교재2", k:"내 새 머그잔 한번 봐 봐. (머그잔에 새겨진) 이건 내 강아지 보보야.", e:"Check out my new mug. That’s my dog, Bobo."},
            {d:22, s:"교재2", k:"오, 멋지다. 내 여동생도 예전에 맞춤형 머그잔 만들어 줬는데.", e:"Oh, that’s nice. My sister made me a personalized mug one time."},
            {d:22, s:"교재2", k:"이 유튜브 채널 본 적 있니?", e:"Have you seen this YouTube channel?"},
            {d:22, s:"교재2", k:"아, 이게 그 캐나다인이랑 한국인 부부 채널이지? 응, 이 채널 너무 좋아.", e:"Oh, is that that Canadian-Korean couple? Yeah, I love that channel."},
            {d:22, s:"교재2", k:"캐나다인 장모님이 늘 이들 부부에게 한국 음식을 해 주시잖아. 우리 엄마도 나한테 그렇게 안 해 주시는데!", e:"The Canadian mother-in-law makes them Korean food all the time. My own mom doesn’t even do that for me!"},
            {d:22, s:"교재2", k:"그러게, 진짜 다정하고 배려심도 넘치셔.", e:"I know, she’s so kind and thoughtful."},
            {d:22, s:"교재3", k:"저희 삼촌은 제2차 세계 대전 때 취사병으로 복무했습니다.", e:"My uncle served as a cook in World War II."},
            {d:22, s:"교재3", k:"혼자 먹으려고 요리하는 것보다 외식하는 게 더 싸서 요리를 잘 안 해요.", e:"I don’t cook much because it’s cheaper to go out and eat than to cook for one person."},
            {d:22, s:"교재3", k:"저는 요리를 전혀 안 해요. 저랑은 안 맞아요.", e:"I don’t cook at all. It’s not my thing."},
            {d:22, s:"교재3", k:"여자들은 요리할 줄 아는 남자를 좋아한다.", e:"Women love a man that can cook."},
            {d:23, s:"대표", k:"아이들이 생긴 후 제가 더 좋은 남편이 되었어요.", e:"Having children has made me a better husband."},
            {d:23, s:"교재1", k:"팬데믹(코로나)으로 온라인 학습이 새로운 표준이 되었다.", e:"The pandemic made online learning the new norm."},
            {d:23, s:"교재1", k:"이 작가의 통찰력 때문에 이 책은 필독서이다.", e:"The author’s insight makes this book a must-read."},
            {d:23, s:"교재1", k:"사업에 실패하고 나서 그는 더 강한 사람이 되었다.", e:"Losing his business made him a stronger person."},
            {d:23, s:"교재1", k:"(프로 선수의 말) 형과의 경쟁이 저를 더 좋은 선수가 되게 했습니다.", e:"Competing with my older brother made me a better athlete."},
            {d:23, s:"교재1", k:"뉴욕시는 식당만으로도 관광객들에게 가장 인기 있는 도시이다.", e:"The restaurants alone make New York City the top city among tourists."},
            {d:23, s:"교재2", k:"그 사람 같이 일하기 참 편해.", e:"He’s super easy to work with."},
            {d:23, s:"교재2", k:"그거 하나만으로도 우리 팀에 적임자지.", e:"That alone makes him a good fit for our team."},
            {d:23, s:"교재2", k:"프로 모델 살까 싶어.", e:"I’m thinking of getting the Pro model."},
            {d:23, s:"교재2", k:"근데 좀 더 비싸잖아.", e:"It’s more expensive, though."},
            {d:23, s:"교재2", k:"그렇긴 하지만, 배터리 수명 때문에 나한테 더 나은 선택이거든.", e:"True, but the battery life makes it a better choice for me."},
            {d:23, s:"교재2", k:"그래, 특히 넌 하루 종일 핸드폰을 하니....", e:"Yeah, especially if you’re on your phone all day…"},
            {d:23, s:"교재3", k:"그 사람 같이 일하기에 너무 짜증 나는 스타일이야.", e:"He is irritating to work with."},
            {d:23, s:"교재3", k:"저랑 술 마시면 재미없을 텐데요.", e:"I’m not much fun to drink with."},
            {d:23, s:"교재3", k:"곱슬머리는 스타일링하기 어렵다.", e:"Curly hair is difficult to style."},
            {d:23, s:"교재3", k:"긴 머리는 말리는 데 한참 걸린다.", e:"Long hair takes a while to dry."},
            {d:24, s:"대표", k:"오늘 억지로 운동하러 갔어요.", e:"I made myself go to the gym today."},
            {d:24, s:"교재1", k:"(여름 캠프에 가기 싫은 아들이 하는 말) 엄마가 저를 강제로 보낼 순 없어요.", e:"You can’t make me go."},
            {d:24, s:"교재1", k:"(야근을 수시로 하는 친구에게 하는 말) 회사가 너를 밤 11시까지 사무실에 붙잡아 둘 수는 없어.", e:"They can’t make you stay at the office until 11 p.m."},
            {d:24, s:"교재1", k:"부모님이 저를 너무 공부시키셔서, 저는 제 아이들에게 아무것도 시키지 않아요.", e:"My parents made me study too much, so I don’t make my children do anything."},
            {d:24, s:"교재1", k:"남편이 저 혼자 그 많은 가방을 들고 가게 했어요.", e:"My husband made me carry all the bags by myself."},
            {d:24, s:"교재1", k:"(채소를 잘 안 먹는 아이의 말) 엄마가 채소를 다 먹게 했어요.", e:"Mom made me finish my vegetables."},
            {d:24, s:"교재2", k:"안녕, 미셸. 회사는 어때?", e:"Good morning, Michelle. How’s work going?"},
            {d:24, s:"교재2", k:"힘들어. 상사가 매주 새로운 온라인 영상을 업로드하라고 해.", e:"It’s tough. My boss makes me upload new online video content every week."},
            {d:24, s:"교재2", k:"네 여자 친구가 벌써 커플 룩 입게 하는 거야?", e:"Your girlfriend is already making you wear matching outfits?"},
            {d:24, s:"교재2", k:"여자 친구가 입으라고 시키는 게 아니야. 내가 입고 싶은 거야.", e:"She doesn’t make me wear them. I want to."},
            {d:24, s:"교재2", k:"만난 지 일주일도 안 됐잖아? 좀 빠르지 않아?", e:"You guys have been dating for less than a week. Isn’t that a little fast?"},
            {d:24, s:"교재2", k:"커플마다 다를 듯. 우리는 좋은데.", e:"Depends on the couple, I guess. We like it."},
            {d:24, s:"교재3", k:"어렸을 때 우리 형이 나한테 자기 방 청소를 시켰어. 무려 10년 동안이나!", e:"Back when we were kids, my brother made me clean his room—for ten years!"},
            {d:24, s:"교재3", k:"부모님은 내가 어릴 때 학교까지 걸어가게 하셨어.", e:"My parents made me walk to school when I was a kid."},
            {d:24, s:"교재3", k:"(갑자기 골절상을 당한 상황) 항공사에 부탁해서 일등석으로 업그레이드 받았어.", e:"I got the airline to upgrade my seat to first class."},
            {d:24, s:"교재3", k:"회사 하루 쉬려고 사라한테 나 대신 근무해 달라고 했어.", e:"I got Sarah to fill in for me at work so I could take the day off."},
            {d:25, s:"대표", k:"교통 체증에 갇혀 있으면 불안해져요.", e:"Sitting in traffic makes me feel anxious."},
            {d:25, s:"교재1", k:"달리기를 하면 기분 전환이 된다.", e:"Running usually makes me feel better. (usually)"},
            {d:25, s:"교재1", k:"너 때문에 내가 기분이 너무 안 좋거든.", e:"You’re making me feel terrible."},
            {d:25, s:"교재1", k:"나를 웃게 만들 수 있는 사람은 여자 친구뿐이다.", e:"My girlfriend is the only one who can make me laugh."},
            {d:25, s:"교재1", k:"공포 영화를 보면 웃게 된다. 이유는 모르겠지만.", e:"Horror movies make me laugh. I don’t know why."},
            {d:25, s:"교재1", k:"수술을 받아 보니 건강이 얼마나 중요한지 알겠더라.", e:"My surgery made me realize how important health is."},
            {d:25, s:"교재2", k:"남편에게 이야기한 게 (기분이 풀리는 데) 도움이 되던가요?", e:"Did talking to your husband help at all?"},
            {d:25, s:"교재2", k:"그렇지도 않아요. 화를 쏟아 냈는데도 기분이 나아지지 않더라고요.", e:"Not really. Venting didn’t make me feel any better."},
            {d:25, s:"교재2", k:"지금까지의 프리랜서 생활은 어때요?", e:"How’s freelance life going so far?"},
            {d:25, s:"교재2", k:"나쁘진 않지만, 그래도 가끔씩은 안정적인 급여가 생각나요.", e:"It’s going OK, but I miss the steady paycheck sometimes."},
            {d:25, s:"교재2", k:"그 마음 알아요.", e:"I get that."},
            {d:25, s:"교재2", k:"혼자 일하다 보니 수입이 들쭉날쭉하면 마음이 불안해질 수 있다는 걸 알겠더라고요.", e:"Working for myself made me realize that unreliable income can make you feel insecure."},
            {d:25, s:"교재3", k:"요즘에는 사람들이 영화관에 그렇게 많이 안 간다.", e:"People don’t go to the movies that often anymore."},
            {d:25, s:"교재3", k:"영화 보러 갈래?", e:"Do you want to go see a movie?"},
            {d:25, s:"교재3", k:"재미있었겠다. 영화 어떤 거 상영 중인지 알아?", e:"That sounds fun. Do you know what’s playing?"},
            {d:25, s:"교재3", k:"마블 영화 신작이 얼마 전에 개봉했어. 같이 보러 갈래?", e:"The new Marvel movie just came out. Do you wanna see it with me?"},
            {d:25, s:"교재3", k:"저는 극장에 가서 영화를 그렇게 즐기는 타입은 아니에요. 전 주로 유튜브만 봐요.", e:"I’m not much of a movie-goer. I usually just stick to YouTube."},

            /* Week 6 (Day 26-30) */
            {d:26, s:"대표", k:"그 팀이 결승에 진출했어요.", e:"They made it to the finals."},
            {d:26, s:"교재1", k:"얘들아, 안녕. (오늘) 점심 식사에 못 갈 것 같아.", e:"Hey, guys. I don’t think I can make it to lunch."},
            {d:26, s:"교재1", k:"우리 내일 파티하는데, 올 수 있겠니?", e:"We’re having a party tomorrow; do you think you can make it?"},
            {d:26, s:"교재1", k:"공항에 간신히 제시간에 도착했더니 비행기가 지연(연착)되었다.", e:"I barely made it to the airport on time, only to have my flight delayed."},
            {d:26, s:"교재1", k:"헬스장에 못 가는 날이면 예민해진다.", e:"I get irritable on days when I can’t make it to the gym."},
            {d:26, s:"교재1", k:"우리 팀이 다음 라운드 진출에 실패했다.", e:"We couldn’t make it to the next round."},
            {d:26, s:"교재2", k:"배스킨라빈스가 10분 있으면 문 닫아.", e:"Baskin-Robbins closes in ten minutes."},
            {d:26, s:"교재2", k:"늦지 않게 도착할 수 있을까?", e:"Do you think we can make it there in time?"},
            {d:26, s:"교재2", k:"미나야, 오늘은 좀 어때?", e:"How’s it going, Mina?"},
            {d:26, s:"교재2", k:"나쁘지 않아, 고마워. 너는?", e:"Not too bad, thanks. How about you?"},
            {d:26, s:"교재2", k:"좋아! 이번 주말에 올 수 있겠니?", e:"Pretty good! Do you think you can make it this weekend?"},
            {d:26, s:"교재2", k:"최선을 다해 볼게! 그나저나 몇 시에 만나지?", e:"I’ll try my best! What time are we meeting, by the way?"},
            {d:26, s:"교재3", k:"꾸준히 연습하면 훌륭한 음악가가 될 거야.", e:"If you keep practicing, you’ll make it as a great musician."},
            {d:26, s:"교재3", k:"그의 이름이 최종 명단에 오르지 못했다.", e:"His name didn’t make it onto the final list."},
            {d:26, s:"교재3", k:"최고의 학생들만이 이 대학에 들어갈 수 있다.", e:"Only the best students make it into this university."},
            {d:26, s:"교재3", k:"(미국 유학 생활 이야기) 매주 포기할까 생각했어요. 그래도 학업을 계속해서 학기 말까지 버틴 게 너무 기쁩니다.", e:"I was thinking of quitting every week. I’m so glad I kept studying, and that I made it to the end of the semester."},
            {d:26, s:"교재3", k:"회의가 2주 조금 더 남아서 참석자 명단을 확인해야 합니다. 참석이 어려울 경우 가급적 빨리 회신 주시기 바랍니다.", e:"The conference is a little over two weeks away, and we need to verify our guest list. Please respond and let me know ASAP if you CANNOT make it."},
            {d:27, s:"대표", k:"그 둘은 정말 잘 어울리는 커플이 될 것 같아요.", e:"Those two would make a great couple."},
            {d:27, s:"교재1", k:"당연히, 당신은 좋은 선생님이 될 거예요.", e:"Of course, you will make a good teacher."},
            {d:27, s:"교재1", k:"그는 훌륭한 배우가 될 자질이 있어요.", e:"He would make a great actor."},
            {d:27, s:"교재1", k:"당신은 훌륭한 코치가 될 거예요.", e:"You’ll make a great coach."},
            {d:27, s:"교재1", k:"이 부지는 훌륭한 정유소가 될 것이다(자리이다).", e:"This site will make an excellent refinery."},
            {d:27, s:"교재1", k:"할머니는 늘 나한테 요리를 배우라고 하셨어요. 안 그러면 좋은 아내가 되지 못할 거라고요.", e:"My grandma always used to tell me to learn to cook. Otherwise, I wouldn’t make a good wife."},
            {d:27, s:"교재2", k:"이 책 도저히 손에서 못 놓겠어. 영화로 만들면 멋질 듯해.", e:"I can’t put this book down. It would make a great movie."},
            {d:27, s:"교재2", k:"정말? 제목이 뭔데? 나도 한번 봐야겠다.", e:"Really? What’s it called? Maybe I’ll check it out."},
            {d:27, s:"교재2", k:"영어 청취 연습을 좀 더 하고 싶어요. 뭐 추천해 주실 게 있나요?", e:"I want to practice my English listening more. Any suggestions?"},
            {d:27, s:"교재2", k:"〈하트스토퍼〉를 한번 봐 보세요. 듣기 연습용 자료로 정말 좋아요.", e:"Try watching Heartstopper. It’ll make great listening material."},
            {d:27, s:"교재2", k:"정말요? 왜 그 드라마를 추천하세요?", e:"Really? Why that show?"},
            {d:27, s:"교재2", k:"언어가 자연스럽고 대화체라, 학습자들에게 안성맞춤입니다.", e:"The language is natural and conversational—perfect for learners."},
            {d:27, s:"교재3", k:"직원들이 여러 군데를 추천해 주더라고.", e:"They suggested a bunch of places to eat at the front desk."},
            {d:27, s:"교재3", k:"뭐 입고 가면 좋을까요?", e:"What do you suggest I wear?"},
            {d:27, s:"교재3", k:"면접에 뭐 입고 가야 해?", e:"What would you recommend I wear to the interview?"},
            {d:28, s:"대표", k:"다 못 먹으면 남은 것을 집으로 가져가면 돼요.", e:"If we can’t finish, we can take the leftovers home."},
            {d:28, s:"교재1", k:"나 대신 이 재킷 세탁소에 좀 가져다줄래?", e:"Could you take this jacket to the dry cleaners for me?"},
            {d:28, s:"교재1", k:"오늘 아침에 아들을 학교에 데려다줬어요.", e:"I took my son to school this morning."},
            {d:28, s:"교재1", k:"바구니에서 원하는 거 아무거나 가져가도 돼.", e:"You can take anything you want from the basket."},
            {d:28, s:"교재1", k:"제가 케이크 두 조각 가져갔어요.", e:"I took two pieces of cake."},
            {d:28, s:"교재1", k:"음악 업로드는 무료인데, 앱에서 매출의 20%를 가져갑니다.", e:"It’s free to upload music, but the app takes 20% of my sales."},
            {d:28, s:"교재2", k:"과자 좀 먹어도 돼?", e:"Can I have some cookies?"},
            {d:28, s:"교재2", k:"당연하지! 마음껏 가져가.", e:"Sure! Take as many as you want."},
            {d:28, s:"교재2", k:"내 첫 번째 앱을 출시할까 해.", e:"I’m thinking of launching my first app."},
            {d:28, s:"교재2", k:"근데 앱 스토어가 매출 한 건당 30%를 떼어 간다는 점은 알아 둬.", e:"Just keep in mind, the app store takes 30% of each sale."},
            {d:28, s:"교재2", k:"정말? 그렇게 많이?", e:"Really? That much?"},
            {d:28, s:"교재2", k:"응, 그래서 많은 개발자들이 그걸 두고 불평해.", e:"Yeah, a lot of developers complain about it."},
            {d:28, s:"교재3", k:"아메리카노 한 잔 주시겠어요?", e:"Can I get an Americano?"},
            {d:28, s:"교재3", k:"냅킨 좀 더 주시겠어요?", e:"Can I get some extra napkins?"},
            {d:28, s:"교재3", k:"맥주 한 잔 주실래요?", e:"Can I get a beer?"},
            {d:28, s:"교재3", k:"(비행기에서) 생선으로 먹을게요.", e:"I’ll have the fish, please."},
            {d:28, s:"교재3", k:"블랙커피로 마시겠습니다.", e:"I’ll have a black coffee."},
            {d:28, s:"교재3", k:"치킨 샐러드로 할게요. (여러 개 중에 선택하는 느낌)", e:"I’ll take the chicken salad."},
            {d:28, s:"교재3", k:"위스키를 온더록스로 할게요. (여러 개의 선택지 중 하나를 고르는 느낌)", e:"I’ll take a whisky on the rocks."},
            {d:29, s:"대표", k:"그 유치원은 5세 미만 아동은 안 받아요.", e:"That kindergarten doesn’t take kids under five."},
            {d:29, s:"교재1", k:"죄송하지만 저희는 예약을 하지 않은 손님은 받지 않습니다.", e:"Sorry, we don’t take walk-ins."},
            {d:29, s:"교재1", k:"(매장 직원의 말) 현재는 신규 고객을 받지 않습니다.", e:"We’re not taking any new clients right now."},
            {d:29, s:"교재1", k:"그 식당은 전화 주문은 더 이상 받지 않는다.", e:"The restaurant doesn’t take phone orders anymore."},
            {d:29, s:"교재1", k:"일부 병원은 네가 가입한 보험은 안 받아.", e:"Some hospitals don’t take your insurance."},
            {d:29, s:"교재1", k:"영수증이 없어도 교환이 가능한가요?", e:"Do you take returns without a receipt?"},
            {d:29, s:"교재2", k:"안녕하세요, 이번 주 금요일에 두 명 예약하고 싶습니다.", e:"Hi, I’d like to make a reservation for two this Friday."},
            {d:29, s:"교재2", k:"죄송한데, 전화로는 예약을 받지 않습니다. 온라인으로 자리를 예약해 주셔야 합니다.", e:"Sorry, we don’t take reservations by phone. You’ll need to book a table online."},
            {d:29, s:"교재2", k:"(기자 회견에서 정부 관계자에게 질문하며) 어려운 경제를 살릴 전략이 뭔가요?", e:"What is your strategy to help the struggling economy?"},
            {d:29, s:"교재2", k:"저와 저희 팀이 계획을 수립 중에 있으며 곧 발표할 겁니다.", e:"My team and I are working on a proposal that we will present very soon."},
            {d:29, s:"교재2", k:"지금 이야기해 주실 수 있을까요?", e:"Can you tell us about it now?"},
            {d:29, s:"교재2", k:"죄송합니다. 지금은 추가 질문을 받을 수 없습니다.", e:"I’m sorry. I can’t take any more questions at this time."},
            {d:29, s:"교재3", k:"오늘 예약이 다 차긴 했는데, 예약 없이 오는 손님 몇 분은 받을 수 있을 듯합니다.", e:"We’re fully booked today, but we might be able to take a few walk-ins."},
            {d:29, s:"교재3", k:"비예약 방문도 환영하지만, 예약을 하는 것이 더 좋습니다.", e:"Walk-ins are welcome, but appointments are preferred."},
            {d:29, s:"교재3", k:"죄송하지만, 저희는 비예약 방문을 받지 않습니다. 미리 예약을 하셔야 합니다.", e:"Sorry, we don’t accept walk-ins. You’ll need to make an appointment."},
            {d:29, s:"교재3", k:"선생님이 오전 9시에서 11시까지만 비예약 환자를 진료합니다.", e:"The doctor only sees walk-ins between 9 and 11 a.m."},
            {d:29, s:"교재3", k:"나는 몇 년 동안 실력이 좋은 특정 디자이너 때문에 유명 미용실에 다녔다. 내 근무 스케줄이 일정하지 않아서, 이 디자이너가 예약 없이 방문해도 받아주는 점이 마음에 들었다. 하지만 시간이 가면서, 디자이너가 내가 예약하는 걸 더 선호한다는 걸 느꼈다. 그래서 그때부터는 웬만하면 예약을 하려고 노력했다.", e:"For a couple years, I used to go to a popular salon for a particular stylist who did a good job. My work schedule was unpredictable, so I liked that she allowed walk-ins. Over time, though, I sensed that my stylist would prefer me to make reservations. I did my best to make a reservation from then on."},
            {d:30, s:"대표", k:"케빈은 누구의 충고도 듣지 않아요.", e:"Kevin doesn’t take advice from anyone."},
            {d:30, s:"교재1", k:"그녀에게 차를 태워 주겠다고 했더니, 좋다고 했다.", e:"I offered her a ride, and she took it."},
            {d:30, s:"교재1", k:"그쪽에서 우리 제안을 수용해서 일정을 변경했습니다.", e:"They took our suggestion and changed the schedule."},
            {d:30, s:"교재1", k:"상사가 진짜로 내 조언을 들을 줄은 몰랐는데, 받아들이더라고!", e:"I didn’t think my boss would actually take my advice, but he did!"},
            {d:30, s:"교재1", k:"이렇게 갑자기 그만두게 되어 죄송합니다만, 이번 기회를 그냥 보낼 수가 없어서요.", e:"I’m sorry I have to quit so suddenly, but I need to take this opportunity."},
            {d:30, s:"교재1", k:"(새로운 일을 시작하는 상황) 지금 모험을 하지 않으면, 너무 늦을 것 같아서요.", e:"If I don’t take a chance now, it might be too late."},
            {d:30, s:"교재2", k:"네가 돈이 궁한게 싫단다. 받아 두렴.", e:"I don’t want you to feel tight on money. Take this."},
            {d:30, s:"교재2", k:"고마워요, 아빠…. 진짜 부탁 안 하고 싶었는데, 감사해요.", e:"Thanks, Dad… I really didn’t want to ask, but I appreciate it."},
            {d:30, s:"교재2", k:"제인이 너를 자기 팀으로 데려가고 싶어 한다고 들었어.", e:"I heard that Jane wants you on her team."},
            {d:30, s:"교재2", k:"응, 월요일에 점심 먹으면서 그 얘기 나눴어.", e:"Yeah, she discussed it with me over lunch on Monday."},
            {d:30, s:"교재2", k:"그 자리를 맡을 거야?", e:"Are you going to take the spot?"},
            {d:30, s:"교재2", k:"이미 맡았어. 다음 주부터 시작해.", e:"I already did. I’m starting next week."},
            {d:30, s:"교재3", k:"내 동생이 생활이 어려워서 돈 좀 빌려줘야겠어요.", e:"My brother can’t make ends meet, so I’m going to lend him some money."},
            {d:30, s:"교재3", k:"우리 부모님은 늘 하루 벌어 하루 먹고사셨다.", e:"My parents always lived paycheck to paycheck."},
            {d:30, s:"교재3", k:"월세가 3개월이나 밀려서, 너랑 유럽에 가는 건 꿈도 못 꿔.", e:"I’m three months behind on rent, so there’s no way I can go with you to Europe."},
            {d:30, s:"교재3", k:"매달 돈 나갈 데가 너무 많아요. 저축은 불가능해요.", e:"I have a lot of bills to pay every month. It’s impossible to save."},

            /* Week 7 (Day 31-35) */
            {d:31, s:"대표", k:"전 좀처럼 화를 내지 않아요.", e:"It takes a lot to make me angry."},
            {d:31, s:"교재1", k:"다이어트를 꾸준히 하려면 많은 절제가 필요하다.", e:"It takes a lot of discipline to stick to a diet."},
            {d:31, s:"교재1", k:"(스쿠버 다이빙을 할 때) 물속에서 편안함을 느끼려면 많은 경험이 있어야 한다.", e:"It takes a lot of experience to feel comfortable underwater."},
            {d:31, s:"교재1", k:"장거리 연애를 지속하려면 많은 믿음이 필요하다.", e:"It takes a lot of trust to maintain a long-distance relationship."},
            {d:31, s:"교재1", k:"첫인상을 망치는 건 정말 순식간이다.", e:"It doesn’t take much to make a bad first impression."},
            {d:31, s:"교재1", k:"내 책을 마무리하기 위해서 정말 많은 노력을 했다.", e:"It took a lot of blood, sweat, and tears to finish my book."},
            {d:31, s:"교재2", k:"사업체를 운영하려면 많은 노력이 필요하지.", e:"It takes a great deal of effort to run a business."},
            {d:31, s:"교재2", k:"맞는 말이야. 사업가로 살면 하루도 쉴 수가 없으니까.", e:"That’s true. You never get a day off as an entrepreneur."},
            {d:31, s:"교재2", k:"빈센트가 맡은 이후로 팀이 훨씬 잘하고 있어요.", e:"The team’s been doing so much better since Vincent took over."},
            {d:31, s:"교재2", k:"맞아요! 이제 모두가 더욱 의욕이 있어 보여요.", e:"Seriously! Everyone seems more motivated now."},
            {d:31, s:"교재2", k:"그는 훌륭한 리더가 될 자질이 확실히 있는 것 같아요.", e:"I really think he has what it takes to be a great leader."},
            {d:31, s:"교재2", k:"동감입니다. 그는 사람들을 화합시키는 방법을 알고 있어요.", e:"Agreed. He knows how to bring people together."},
            {d:31, s:"교재3", k:"그 사람 지금 극심한 고통을 겪고 있습니다.", e:"He’s in a great deal of pain."},
            {d:31, s:"교재3", k:"그 사람 지금 많이 아파요.", e:"He’s in a lot of pain."},
            {d:31, s:"교재3", k:"저희 어머니는 정말 지혜로운 분이셨어요.", e:"My mother had a great deal of wisdom."},
            {d:31, s:"교재3", k:"제 사촌이 칠레에서 오래 생활을 해서 스페인어를 할 줄 알아요.", e:"My cousin spent a great deal of time in Chile, so she speaks Spanish."},
            {d:31, s:"교재3", k:"우리 아직 개발자 한 명 더 채용해야 하는데, 아는 사람 있어?", e:"We still need to hire another developer. Do you know anyone?"},
            {d:31, s:"교재3", k:"응, 있어. 내 사촌이 개발자인데 지식과 경험이 풍부해.", e:"Yeah, actually. My cousin is a developer with a great deal of knowledge and experience."},
            {d:32, s:"대표", k:"그 책 다 읽는 데 몇 달이 걸렸어요.", e:"It took me months to finish reading that book."},
            {d:32, s:"교재1", k:"이 책을 쓰는 데 몇 달이 걸렸다.", e:"This book took me months to write."},
            {d:32, s:"교재1", k:"이 음식 준비하는 데 서너 시간이 걸렸다.", e:"This meal took me three or four hours to prepare."},
            {d:32, s:"교재1", k:"우리 집에서 강까지 걸어서 가는 데 겨우 30분이 걸린다.", e:"It only takes 30 minutes to walk to the river from my place."},
            {d:32, s:"교재1", k:"천 달러 모으는 데 얼마나 걸릴까?", e:"How long would it take you to save $1,000?"},
            {d:32, s:"교재1", k:"7년이 걸렸지만 나는 결국 대학을 졸업했다.", e:"It took me seven years, but I eventually graduated from college."},
            {d:32, s:"교재2", k:"그냥 목도리 하나 사지 그래? (직접) 뜨개질하는 건 시간 낭비잖아.", e:"Why don’t you just buy a scarf? Knitting is a waste of time."},
            {d:32, s:"교재2", k:"목도리 뜨는 데 몇 시간이 걸리는 건 맞아. 하지만 마음이 정말 편안해져.", e:"It’s true that knitting a scarf takes me hours, but it’s so relaxing."},
            {d:32, s:"교재2", k:"엘리베이터에 에어컨 설치한다는 거 들었어요?", e:"Did you hear they’re putting air conditioners in the elevators?"},
            {d:32, s:"교재2", k:"네, 여름에 딱 맞춰서요! 여름에는 엘리베이터 안이 너무 숨 막혀요.", e:"Yeah, just in time! It gets so stuffy in there during summer."},
            {d:32, s:"교재2", k:"맞아요. 작년에는 너무 힘들더군요.", e:"Seriously. Last year was unbearable."},
            {d:32, s:"교재2", k:"이게 이렇게 오래 걸릴 일인가 싶어요.", e:"I can’t believe it took them this long."},
            {d:32, s:"교재3", k:"그림 그리기가 마음이 편해지는 취미라고들 한다.", e:"Some say painting is a relaxing hobby."},
            {d:32, s:"교재3", k:"그 사람이랑 이야기하면 마음이 편해진다.", e:"Talking to him is very calming."},
            {d:32, s:"교재3", k:"농장에서 일하는 건 힘들지만, 육체노동을 하면 마음이 편하다.", e:"Working on the farm is demanding, but it puts me at ease to work with my hands."},
            {d:32, s:"교재3", k:"오늘은 대체로 스트레스 없는 하루였어. 별다른 이슈가 없었거든.", e:"Today was pretty stress-free. No issues at all."},
            {d:32, s:"교재3", k:"나는 드럼을 치면 스트레스가 풀린다.", e:"Playing the drums is a stress reliever for me."},
            {d:32, s:"교재3", k:"캐모마일 차는 스트레스를 완화하는 데 도움이 된다.", e:"Some chamomile tea can help ease your stress."},
            {d:32, s:"교재3", k:"(엄마가 자녀에게 하는 말) 네가 하루 종일 집 안을 그렇게 계속 돌아다니는 걸 보니까 정신 없다. 좀 앉아서 차분하게 있으렴.", e:"Watching you walk around the house all day stresses me out. Please sit down and relax for a while."},
            {d:33, s:"대표", k:"그녀가 기분 상해하더군요.", e:"She didn’t take it well."},
            {d:33, s:"교재1", k:"칭찬으로 받아들일게.", e:"I’ll take that as a compliment."},
            {d:33, s:"교재1", k:"아들이 시험에 떨어졌는데, 그 일로 몹시 힘들어하고 있어요.", e:"My son failed his test, and he’s taking it really hard."},
            {d:33, s:"교재1", k:"타라는 큰 수술이 필요하다는 사실을 알게 됐어요. 그래도 그 소식을 꽤 담담하게 받아들였어요.", e:"Tara learned she needs major surgery. She took the news pretty well, though."},
            {d:33, s:"교재1", k:"우울증은 가볍게 볼 것이 아니다.", e:"Depression cannot be taken lightly."},
            {d:33, s:"교재1", k:"당신은 자신의 건강을 대수롭지 않게 생각하시는 듯하네요.", e:"It doesn’t seem like you’re taking your health seriously."},
            {d:33, s:"교재2", k:"스티브 왜 저래?", e:"What’s wrong with Steve?"},
            {d:33, s:"교재2", k:"악플러들 때문이지 뭐. 그는 유튜브 댓글에 아주 민감하거든.", e:"Internet trolls. He takes YouTube comments so personally."},
            {d:33, s:"교재2", k:"그래서, 잭, 지금까지 서울에서 살아 보니 어때요?", e:"So, Jack, how do you like living in Seoul so far?"},
            {d:33, s:"교재2", k:"그렇게 좋지는 않아요. 새로운 직장이 스트레스가 정말 심해요.", e:"Not great. My new job is really stressful."},
            {d:33, s:"교재2", k:"자녀가 있으시죠? 아마 전학도 해야 했겠네요.", e:"You have kids, right? They probably had to change schools."},
            {d:33, s:"교재2", k:"맞아요, 그래도 애들은 오히려 저보다 이사에 더 잘 적응하고 있어요. 벌써 새 친구들도 생긴걸요.", e:"Right, but they’re taking the move better than me. They already have new friends."},
            {d:33, s:"교재3", k:"영화 어땠어? (재미있었지?)", e:"How did you like the movie?"},
            {d:33, s:"교재3", k:"음식 어때? (괜찮지?)", e:"How do you like your food?"},
            {d:33, s:"교재3", k:"유튜브 영상 만드는 거 재미있어요?", e:"How do you like making YouTube videos?"},
            {d:33, s:"교재3", k:"(유럽 여행에서 돌아온 친구에게) 런던은 좋았어?", e:"How did you like London?"},
            {d:33, s:"교재3", k:"잭, 은퇴 생활은 어때요?", e:"How do you like retirement, Jack?"},
            {d:33, s:"교재3", k:"일하는 것보단 낫긴 한데, 예상했던 것보다 훨씬 지루하네요.", e:"It’s better than working, but more boring than I expected."},
            {d:34, s:"대표", k:"저는 회사에서 일반 행정 업무를 많이 해요.", e:"I do a lot of admin work at my job."},
            {d:34, s:"교재1", k:"나는 주말에 플라잉 요가를 한다.", e:"I do flying yoga on weekends."},
            {d:34, s:"교재1", k:"제 아내는 아침에 한 시간씩 머리를 손질해요.", e:"My wife spends an hour doing her hair in the morning."},
            {d:34, s:"교재1", k:"빨리 검색해 보고 찾은 것을 이메일로 보내 드릴게요.", e:"Let me do a quick search, and I’ll email you what I find."},
            {d:34, s:"교재1", k:"남편이 출장 갈 때는 내가 장을 본다.", e:"I do the grocery shopping when my husband is out of town."},
            {d:34, s:"교재1", k:"(경찰관의 말) 뭔가 긴박한 일이 벌어지고 나면, 서류 작업이 산더미입니다.", e:"After anything exciting happens, I have to do loads of paperwork."},
            {d:34, s:"교재2", k:"오늘 오후에 시간 돼?", e:"Are you free this afternoon?"},
            {d:34, s:"교재2", k:"안 될 것 같은데. 미용실 예약도 있고, 그다음엔 손톱 손질도 받을 거라서.", e:"I’m afraid not. I have a hair appointment, then I’m going to get my nails done."},
            {d:34, s:"교재2", k:"세금 신고하는 거 너무 싫어. 벌써 5월이라니 믿어지지 않아.", e:"I hate doing my taxes. I can’t believe it’s May already."},
            {d:34, s:"교재2", k:"정말 괜찮은 세무사 아는데. 명함 줄까?", e:"I have a great tax accountant. Would you like her card?"},
            {d:34, s:"교재2", k:"응, 명함은 받을게. 근데 내가 할 수 있는 걸 돈을 내고 한다는 게 좀 그래서.", e:"Sure, I’ll take it, but I don’t like paying for things I can do myself."},
            {d:34, s:"교재2", k:"대신 스트레스를 많이 줄일 수 있을 거야. 그건 돈하고 바꿀 수 없잖아, 친구야.", e:"It would save you a lot of stress. That is priceless, my friend."},
            {d:34, s:"교재3", k:"여자 친구 생일 선물로 멋진 시계를 사 줬다.", e:"I got my girlfriend a nice watch for her birthday."},
            {d:34, s:"교재3", k:"온라인 뱅킹으로 은행에 가는 수고를 덜 수 있다.", e:"Online banking saves us a trip to the bank."},
            {d:34, s:"교재3", k:"오는 길에 커피 좀 사다 줄 수 있어요?", e:"Do you mind grabbing me some coffee on your way?"},
            {d:34, s:"교재3", k:"그가 집에 태워 주겠다고 했어요.", e:"He offered me a ride home."},
            {d:35, s:"대표", k:"골프 치는 거 별로 안 좋아하지만, 일 때문에 어쩔 수 없이 해야 해요.", e:"I don’t like to play golf, but I have to for my job."},
            {d:35, s:"교재1", k:"나는 TV 보면서 뭘 자주 먹곤 한다.", e:"I like to eat something while I watch TV."},
            {d:35, s:"교재1", k:"우리 아들은 진흙에서 노는 걸 좋아한다.", e:"My son likes to play in the mud."},
            {d:35, s:"교재1", k:"우리 고양이는 테이블에 있는 물건을 툭 쳐서 떨어뜨리는 버릇이 있다.", e:"My cat likes to knock things off the table."},
            {d:35, s:"교재1", k:"공부하는 게 재미있긴 하지만, 의료 분야는 내 적성에 안 맞는다.", e:"I like to study, but I’m not cut out for the medical field."},
            {d:35, s:"교재1", k:"그녀는 저녁 식사 중에 정치 이야기를 자주 꺼낸다.", e:"She likes to bring up politics at dinner."},
            {d:35, s:"교재2", k:"민호야, 운동 즐겨 하니?", e:"Do you like to exercise, Minho?"},
            {d:35, s:"교재2", k:"아니, 그래도 여름 전에 식스 팩을 만들고 싶어.", e:"No, but I want to get a 6-pack before summer."},
            {d:35, s:"교재2", k:"나 영어 과외 선생님 새로 구해야 해.", e:"I need to find a new English tutor."},
            {d:35, s:"교재2", k:"왜? 지금 선생님이랑 시작한 지 얼마 안 되지 않았어?", e:"Why? Didn’t you just start with your current teacher?"},
            {d:35, s:"교재2", k:"응, 근데 이 분이 수업 시간 내내 정치 얘기하는 걸 좋아해서 더 이상 못 참겠어.", e:"Yeah, but he likes to spend most of the session talking about politics, and I can’t stand it anymore."},
            {d:35, s:"교재2", k:"오, 그건 진짜 아니다.", e:"Oh, that does sound awful."},
            {d:35, s:"교재3", k:"난 말차 안 좋아해.", e:"I don’t like matcha."},
            {d:35, s:"교재3", k:"난 밤에 밖에 나가는 거 안 좋아해.", e:"I don’t like going out at night."},
            {d:35, s:"교재3", k:"이 머그잔 너무 싫어.", e:"I hate this mug."},
            {d:35, s:"교재3", k:"메뉴에 아무런 표시도 없이 치즈피자 위에 옥수수를 얹으면 정말 싫어.", e:"I hate when they put corn on cheese pizza without any warning on the menu."},
            {d:35, s:"교재3", k:"당신 부모님이랑 다음 달에 캠핑 가는 거 어때?", e:"How about going camping with your parents next month?"},
            {d:35, s:"교재3", k:"안 돼, 우리 부모님은 야외 활동 하는 거 별로 안 좋아하셔. 투덜거리기만 하실 거야.", e:"Nah, my parents dislike the outdoors. They’d just complain."},

            /* Week 8 (Day 36-40) - 신규 추가 */
            {d:36, s:"대표", k:"한동안 못 봤네요.", e:"I haven’t seen you for a while."},
            {d:36, s:"교재1", k:"저 새 봤어?", e:"Did you see that bird?"},
            {d:36, s:"교재1", k:"저기 아래쪽에 지하철역이 보이는 것 같은데.", e:"I think I see a subway station down that way."},
            {d:36, s:"교재1", k:"최근에 닉 만난 적 있어?", e:"Have you seen Nick lately?"},
            {d:36, s:"교재1", k:"비 오는지 확인 좀 해 줄래?", e:"Can you see if it’s raining?"},
            {d:36, s:"교재1", k:"토요일에 시간 되는지 한번 볼게요.", e:"Let me see if I’m free on Saturday."},
            {d:36, s:"교재2", k:"이 주차장은 너무 넓어서 정말 헷갈려.", e:"These huge parking lots are so confusing."},
            {d:36, s:"교재2", k:"아, 우리 차 보이는 것 같아. 빨간색 바로 옆에. 너도 보이지?", e:"Oh, I think I see our car. Next to the red one. Do you see it?"},
            {d:36, s:"교재2", k:"토니 말로는 금요일 밤 파티에서 널 봤다는데.", e:"Tony said he saw you at the party Friday night."},
            {d:36, s:"교재2", k:"응, 우연히 그를 보게 돼서 놀랐어. 고등학교 이후로 얼굴 본 적 없거든.", e:"Yeah, I was surprised to run into him. I hadn’t seen him since high school."},
            {d:36, s:"교재2", k:"언제 한번 다 같이 모여서 밀린 이야기하자.", e:"We should all get together sometime and catch up."},
            {d:36, s:"교재2", k:"그럼 너무 좋지. 나도 끼워 줘.", e:"That’d be great. Count me in."},
            {d:36, s:"교재3", k:"지금 사귀는 사람 있나요?", e:"Are you seeing anyone right now?"},
            {d:36, s:"교재3", k:"아, 이제 무슨 말인지 알겠네요.", e:"Oh, I see what you mean now."},
            {d:36, s:"교재3", k:"이 두 가지 옵션의 차이점을 아시겠어요?", e:"Do you see the difference between the two options?"},
            {d:36, s:"교재3", k:"내일 비가 올 것 같은데, 한번 보자고.", e:"I think it’s going to rain tomorrow, but we’ll see."},
            {d:36, s:"교재3", k:"우리 할머니는 다음 주에 아흔이 되신다. 평생을 살면서 정말 많은 일을 겪으셨다.", e:"My grandma turns 90 next week. She has definitely seen a lot in her lifetime."},
            {d:37, s:"대표", k:"그 집 주인 잘 알아요.", e:"I know the owner."},
            {d:37, s:"교재1", k:"여기서 멀지 않은 괜찮은 곳을 알아.", e:"I know a good place not far from here."},
            {d:37, s:"교재1", k:"괜찮은 식당 아는 데 있어. 예전부터 한번 가 보고 싶었거든.", e:"I know of a good place. I’ve been wanting to try it."},
            {d:37, s:"교재1", k:"난 텍사스 출신 사람들을 많이 알아.", e:"I know many people from Texas."},
            {d:37, s:"교재1", k:"텍사스 출신 유명한 사람들 몇 명 알고는 있지.", e:"I know of some famous people from Texas."},
            {d:37, s:"교재1", k:"고등학교 때는 너를 잘 몰랐지만, 너의 존재는 알고 있었지.", e:"I didn’t know you in high school, but I knew of you."},
            {d:37, s:"교재2", k:"신디 알아?", e:"Do you know Cindy?"},
            {d:37, s:"교재2", k:"알긴 알지. 네가 항상 그 친구 이야기를 하니까. 하지만 만난 적은 없어.", e:"I know of her, because you always talk about her, but I haven’t met her."},
            {d:37, s:"교재2", k:"이 의류 브랜드 들어 봤어?", e:"Do you know of this clothing brand?"},
            {d:37, s:"교재2", k:"사람들이 입고 다니는 건 봤는데, 이름은 몰랐어.", e:"I’ve seen people wearing it, but I didn’t know the name."},
            {d:37, s:"교재2", k:"응, 요즘 아주 핫해.", e:"Yeah, it’s pretty trendy these days."},
            {d:37, s:"교재2", k:"네가 좋아하는 스타일인 듯. 너한테 잘 어울릴 것 같아.", e:"It seems like your style. It would suit you."},
            {d:37, s:"교재3", k:"어제 우리 동네에서 사람들이 뉴스 촬영하고 있는 것을 봤어.", e:"I saw some people filming a news report on my street yesterday."},
            {d:37, s:"교재3", k:"자전거 타고 가는 저 여자애 봤어? 내가 아는 애 같은데.", e:"Did you see that girl riding the bike? I think I know her."},
            {d:37, s:"교재3", k:"오늘 점심 때 샐리가 네 험담하는 것을 들었어.", e:"I heard Sally talking behind your back at lunch today."},
            {d:37, s:"교재3", k:"어젯밤에 누가 소리를 지르는 걸 들었어.", e:"I heard someone screaming last night."},
            {d:37, s:"교재3", k:"뭐 타는 냄새 안 나니?", e:"Do you smell something burning?"},
            {d:37, s:"교재3", k:"잠깐만, 요리하는 냄새가 나네. 냄새 너무 좋다.", e:"Wait, I smell something cooking. Smells great."},
            {d:37, s:"교재3", k:"어젯밤에 뭔가 내 몸에 기어다니는 느낌이 들었어요.", e:"I felt something crawling on me last night."},
            {d:37, s:"교재3", k:"건물 흔들리는 거 느꼈어?", e:"Did you feel the building shaking?"},
            {d:38, s:"대표", k:"이 다이어트는 저한테 효과 만점이었어요.", e:"This diet really worked for me."},
            {d:38, s:"교재1", k:"이 학습 방법은 아이들에게 너무 잘 맞는다.", e:"This learning method works really well with kids."},
            {d:38, s:"교재1", k:"화요일 시간 가능하세요?", e:"Does Tuesday work for you?"},
            {d:38, s:"교재1", k:"침이 나한테는 정말 잘 맞아. 너도 한번 맞아 보는 게 어때?", e:"Acupuncture really works for me. Why don’t you give it a shot?"},
            {d:38, s:"교재1", k:"강남이나 용산에서 봐도 돼. 너 편한 대로 해.", e:"We can meet in Gangnam or Yongsan— wherever works best for you."},
            {d:38, s:"교재1", k:"간헐적 단식을 해 봤는데, 나한테는 별로였어.", e:"I tried intermittent fasting, but it didn’t work for me."},
            {d:38, s:"교재2", k:"내일 점심 전에 나랑 공부할래?", e:"Do you want to study with me before lunch tomorrow?"},
            {d:38, s:"교재2", k:"아니, 괜찮아. 난 밤늦게 공부하는 게 더 맞거든.", e:"No, thanks. Studying late at night works better for me."},
            {d:38, s:"교재2", k:"토요일에 하이킹 가자고 할까 생각 중이었어.", e:"I was thinking we could go hiking on Saturday."},
            {d:38, s:"교재2", k:"우리 4시에 결혼식 가야 하지 않아?", e:"Don’t we have a wedding to go to at 4?"},
            {d:38, s:"교재2", k:"아, 맞다. 그럼 일요일은 어때?", e:"Oh, right. Then maybe Sunday instead?"},
            {d:38, s:"교재2", k:"응, 그게 더 나을 것 같아.", e:"Yeah, that works better."},
            {d:38, s:"교재3", k:"내가 마라톤을 완주할 수 있을지 모르겠어.", e:"I’m not sure I could finish a marathon."},
            {d:38, s:"교재3", k:"지금 당장은 아니더라도, 훈련하면 할 수도 있을 거야. 도전해 봐!", e:"Maybe not today, but if you trained for it, you could. Give it a shot!"},
            {d:38, s:"교재3", k:"이 무료 그림 그리기 수업 들어야 할까?", e:"Should I take this free painting class?"},
            {d:38, s:"교재3", k:"뭘 망설여. 한번 해 봐.", e:"Why not? Go for it."},
            {d:38, s:"교재3", k:"이 맥주 정말 맛있어. 여기, 한번 마셔 봐.", e:"This beer is really good. Here, have a try."},
            {d:38, s:"교재3", k:"이 피클 단지 못 열겠어.", e:"I can’t open this pickle jar."},
            {d:38, s:"교재3", k:"내가 한번 해 볼게. 오늘 헬스장 다녀왔거든.", e:"Let me have a try— I went to the gym today."},
            {d:39, s:"대표", k:"데이트 방식이 더 이상 그렇지가 않아요. 시대가 바뀌었어요.", e:"Dating doesn’t work that way anymore. Times have changed."},
            {d:39, s:"교재1", k:"누군가가 너를 억지로 사랑하게 만들 수는 없어. 사랑이라는 게 그런 게 아니잖아.", e:"You can’t force someone to love you— it doesn’t work that way."},
            {d:39, s:"교재1", k:"반바지를 입고 사무실에 간다? 여기서는 안 됩니다.", e:"Wearing shorts to the office? That won’t work around here."},
            {d:39, s:"교재1", k:"죄송한데, 저희 앱은 그렇게 운영되지 않습니다.", e:"Sorry, but the app doesn’t work that way."},
            {d:39, s:"교재1", k:"무료 환승이 택시에는 적용이 안 되는구나.", e:"I couldn’t use my free transportation transfer with a taxi."},
            {d:39, s:"교재1", k:"응, 교통 카드를 그렇게는 쓸 수 없어.", e:"Yeah, transportation cards don’t work that way."},
            {d:39, s:"교재2", k:"은퇴하면 삶이 좀 편해질 줄 알았더니.", e:"I thought life would get easier after retiring."},
            {d:39, s:"교재2", k:"삶이라는 게 꼭 그렇지가 않잖아. 상황이 변한다고 더 편해지는 건 아니지.", e:"Life doesn’t really work that way. Things change, but they don’t get easier."},
            {d:39, s:"교재2", k:"이 차 나오기를 2년이나 기다렸어요.", e:"I’ve been waiting two years for this car to come out."},
            {d:39, s:"교재2", k:"그 말을 들으니 기쁘네요. 물론 오늘까지 계약금 3백만 원을 내셔야 합니다.", e:"We’re happy to hear that. Of course, you’ll need to make a 3 million won deposit today."},
            {d:39, s:"교재2", k:"괜찮습니다. 그런데 무슨 일 생기면 계약금을 돌려받을 수 있나요?", e:"Not a problem. If something comes up, though, is it possible for me to get my deposit back?"},
            {d:39, s:"교재2", k:"안타깝지만 그건 안 됩니다. 계약금은 환불이 안 됩니다.", e:"It doesn’t work that way, unfortunately. All deposits are non-refundable."},
            {d:39, s:"교재3", k:"오늘 조금 일찍 퇴근해 봐야 할지도 모르겠어요. 괜찮을까요?", e:"I might need to leave a bit early today. Would that be OK?"},
            {d:39, s:"교재3", k:"괜찮아요. 퇴근 전에 팀원들에게 말만 해 줘요.", e:"Not a problem. Just let the team know before you go."},
            {d:39, s:"교재3", k:"데리러 와 줘서 고마워!", e:"Thanks for picking me up!"},
            {d:39, s:"교재3", k:"천만에. 어차피 가는 길이었는걸.", e:"Not a problem. I was on my way anyway."},
            {d:39, s:"교재3", k:"이 바지 다른 사이즈로 교환해야겠어요.", e:"I need to exchange these pants for a different size."},
            {d:39, s:"교재3", k:"영수증 있으신가요?", e:"Do you have the receipt?"},
            {d:39, s:"교재3", k:"아니요, 선물로 받은 거라서요.", e:"No, they were a gift."},
            {d:39, s:"교재3", k:"괜찮습니다. 이 태그만 스캔하면 돼요.", e:"Not a problem. I just need to scan this tag."},
            {d:40, s:"대표", k:"여행 잘 다녀오셨다니 다행입니다.", e:"I’m glad to hear your trip went well."},
            {d:40, s:"교재1", k:"데이트는 정말 좋았어요. 이번 주 금요일에 다시 만나기로 이미 약속했어요.", e:"The date went great. We already have plans to meet again this Friday."},
            {d:40, s:"교재1", k:"내일 회의가 잘 되기를 바랍니다.", e:"I hope the meeting goes well tomorrow."},
            {d:40, s:"교재1", k:"면접이 거의 완벽하게 잘 된 것 같다.", e:"I think the interview went almost perfectly."},
            {d:40, s:"교재1", k:"수술이 잘 됐다고 들었어요.", e:"I heard the surgery went well."},
            {d:40, s:"교재1", k:"런던에서의 생활이 지금까지는 순조롭다.", e:"Life in London is going well so far."},
            {d:40, s:"교재2", k:"다니엘, 어젯밤 네 사촌의 친구랑은 어땠어?", e:"Daniel, how did it go last night with your cousin’s friend?"},
            {d:40, s:"교재2", k:"세 번째 만나는 거였는데 괜찮았어. 근데 그녀에 대해 아직 확신이 안 서는 게 있긴 해.", e:"It was the third date, and it went OK, but there’s still something about her that I’m not sure about."},
            {d:40, s:"교재2", k:"앤드루, 새 학기는 어때요?", e:"Andrew, how’s your new semester going?"},
            {d:40, s:"교재2", k:"지금까지는 별 탈 없이 잘 되고 있어요.", e:"It’s going smoothly so far."},
            {d:40, s:"교재2", k:"수업 많이 맡고 있나요?", e:"Are you teaching a lot of classes?"},
            {d:40, s:"교재2", k:"네, 하지만 내용을 잘 알고 있어서, 꽤 수월하게 느껴져요.", e:"Yes, but I know the material really well, so it feels pretty light."},
            {d:40, s:"교재3", k:"옛날 팝송에는 왠지 모르게 마음이 편안해지는 무언가가 있다.", e:"There is something about old pop songs that I find so comforting."},
            {d:40, s:"교재3", k:"라면을 먹으면 이상하게 속이 불편해진다.", e:"There’s something about ramyeon that upsets my stomach."},
            {d:40, s:"교재3", k:"이곳엔 무언가 익숙한 느낌이 있다.", e:"Something about this place feels so familiar."},
            {d:40, s:"교재3", k:"그 사람 태도가 뭔가 거슬린다.", e:"Something about his attitude bothers me."},
            {d:40, s:"교재3", k:"노트북은 나랑 안 맞다. 키보드가 뭔가 너무 불편하다.", e:"Laptops aren’t really for me. Something about the keyboards is super uncomfortable."}
        ];

        allData.forEach((item, index) => item.id = index + 1);
        const sourceMap = { "대표": "대표예제", "교재1": "Model examples", "교재2": "Small talk", "교재3": "Further studies" };

        let selectedWeekMode = 'w8';
        let quizPool = [];
        let currentIndex = 0;
        let starredIds = new Set();

        function selectWeek(mode) {
            selectedWeekMode = mode;
            document.querySelectorAll('.week-tab').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(`tab-${mode}`).classList.add('active-tab');
            
            const titleEl = document.getElementById('selected-week-title');
            if(mode === 'w1') titleEl.innerText = "WEEK 1 (Day 1-5)";
            else if(mode === 'w2') titleEl.innerText = "WEEK 2 (Day 6-10)";
            else if(mode === 'w3') titleEl.innerText = "WEEK 3 (Day 11-15)";
            else if(mode === 'w4') titleEl.innerText = "WEEK 4 (Day 16-20)";
            else if(mode === 'total1') titleEl.innerText = "WEEK 1-4 (Month 1 Total)";
            else if(mode === 'w5') titleEl.innerText = "WEEK 5 (Day 21-25)";
            else if(mode === 'w6') titleEl.innerText = "WEEK 6 (Day 26-30)";
            else if(mode === 'w7') titleEl.innerText = "WEEK 7 (Day 31-35)";
            else if(mode === 'w8') titleEl.innerText = "WEEK 8 (Day 36-40)";
            else if(mode === 'total2') titleEl.innerText = "WEEK 5-8 (Month 2 Accumulated)";
        }

        function unlockAudio() {
            if ('speechSynthesis' in window) {
                const msg = new SpeechSynthesisUtterance("");
                window.speechSynthesis.speak(msg);
                window.speechSynthesis.cancel();
            }
        }

        function startQuiz(difficulty) {
            unlockAudio();

            let weekFiltered = [];
            if (selectedWeekMode === 'w1') weekFiltered = allData.filter(item => item.d >= 1 && item.d <= 5);
            else if (selectedWeekMode === 'w2') weekFiltered = allData.filter(item => item.d >= 6 && item.d <= 10);
            else if (selectedWeekMode === 'w3') weekFiltered = allData.filter(item => item.d >= 11 && item.d <= 15);
            else if (selectedWeekMode === 'w4') weekFiltered = allData.filter(item => item.d >= 16 && item.d <= 20);
            else if (selectedWeekMode === 'total1') weekFiltered = allData.filter(item => item.d >= 1 && item.d <= 20);
            else if (selectedWeekMode === 'w5') weekFiltered = allData.filter(item => item.d >= 21 && item.d <= 25);
            else if (selectedWeekMode === 'w6') weekFiltered = allData.filter(item => item.d >= 26 && item.d <= 30);
            else if (selectedWeekMode === 'w7') weekFiltered = allData.filter(item => item.d >= 31 && item.d <= 35);
            else if (selectedWeekMode === 'w8') weekFiltered = allData.filter(item => item.d >= 36 && item.d <= 40);
            else if (selectedWeekMode === 'total2') weekFiltered = allData.filter(item => item.d >= 21 && item.d <= 40);

            let finalFiltered = [];
            if (difficulty === 'mild') {
                finalFiltered = weekFiltered.filter(item => item.s === '대표' || item.s === '교재1');
            } else {
                finalFiltered = [...weekFiltered];
            }

            if (finalFiltered.length === 0) {
                alert("선택한 범위에 데이터가 없습니다.");
                return;
            }

            quizPool = finalFiltered.sort(() => Math.random() - 0.5).slice(0, 10);
            currentIndex = 0;
            starredIds.clear();
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            showQuestion();
        }

        function showQuestion() {
            const item = quizPool[currentIndex];
            const card = document.getElementById('card-container');
            const star = document.getElementById('star-btn');
            
            card.classList.remove('flipped');
            star.classList.remove('star-checked');
            document.getElementById('next-btn').classList.add('hidden');
            
            document.getElementById('q-korean').innerText = item.k;
            document.getElementById('q-english').innerText = item.e;
            document.getElementById('q-info').innerText = `Day ${String(item.d).padStart(3, '0')} - ${sourceMap[item.s]}`;
            document.getElementById('progress-text').innerText = `${currentIndex + 1} / ${quizPool.length}`;
        }

        function flipCard() {
            document.getElementById('card-container').classList.add('flipped');
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function toggleStar(e) {
            e.stopPropagation();
            const item = quizPool[currentIndex];
            const starBtn = document.getElementById('star-btn');
            if (starredIds.has(item.id)) {
                starredIds.delete(item.id);
                starBtn.classList.remove('star-checked');
            } else {
                starredIds.add(item.id);
                starBtn.classList.add('star-checked');
            }
        }

        function playTTS() {
            const text = document.getElementById('q-english').innerText;
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const msg = new SpeechSynthesisUtterance(text);
                msg.lang = 'en-US';
                msg.rate = 0.9;
                window.speechSynthesis.speak(msg);
            }
        }

        function nextQuestion() {
            currentIndex++;
            if (currentIndex < quizPool.length) showQuestion();
            else showResults();
        }

        function showResults() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            const listEl = document.getElementById('starred-list');
            listEl.innerHTML = '<p class="text-[10px] text-slate-400 font-black mb-4 uppercase tracking-widest text-center">⭐ 다시 학습할 문장</p>';
            
            const starredItems = quizPool.filter(item => starredIds.has(item.id));
            if (starredItems.length === 0) {
                listEl.innerHTML += '<p class="text-sm text-slate-400 py-12 text-center font-bold">완벽합니다! 체크한 문장이 없네요.</p>';
            } else {
                starredItems.forEach(item => {
                    const div = document.createElement('div');
                    div.className = "p-5 bg-indigo-50 rounded-2xl border border-indigo-100 mb-3";
                    div.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-[9px] bg-indigo-500 text-white px-1.5 py-0.5 rounded font-black tracking-tighter">DAY ${item.d}</span>
                            <span class="text-[9px] text-indigo-400 font-bold italic">${sourceMap[item.s]}</span>
                        </div>
                        <p class="text-xs text-slate-500 mb-1 leading-relaxed">${item.k}</p>
                        <p class="text-sm font-black text-indigo-900 leading-snug">${item.e}</p>
                    `;
                    listEl.appendChild(div);
                });
            }
        }
        
        // 초기 시작 탭을 W8로 설정
        window.onload = () => {
            selectWeek('w8');
        }
    </script>
</body>
</html>
