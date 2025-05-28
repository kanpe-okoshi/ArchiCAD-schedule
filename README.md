<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ArchiCAD関連イベント・勉強会</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .event-card {
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
        }
        .event-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .filter-select {
            min-width: 150px;
        }
         /* Chart.js Tooltip Customization */
        .chartjs-tooltip {
            opacity: 1;
            position: absolute;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            border-radius: 3px;
            -webkit-transition: all .1s ease;
            transition: all .1s ease;
            pointer-events: none;
            -webkit-transform: translate(-50%, 0);
            transform: translate(-50%, 0);
            padding: 4px 6px;
            font-size: 12px;
        }

        .chartjs-tooltip-key {
            display: inline-block;
            width: 10px;
            height: 10px;
            margin-right: 5px;
        }
    </style>
</head>
<body class="bg-stone-100 text-stone-800">
    <div class="container mx-auto p-4 sm:p-6 lg:p-8 max-w-7xl">
        <header class="mb-8 text-center">
            <h1 class="text-3xl sm:text-4xl font-bold text-sky-700">ArchiCAD関連イベント・勉強会</h1>
            <p class="mt-2 text-stone-600">このページでは、現在の月とその前後1か月（例: 2025年4月、5月、6月）に開催予定のArchiCADおよびBIM関連のイベントや勉強会を一覧でご確認いただけます。フィルターを使って、開催地、イベントの種類、そして開催月で絞り込むことができます。興味のあるイベントを見つけて、ぜひご参加ください。</p>
        </header>

        <div class="filters mb-8 p-4 bg-white rounded-lg shadow-md">
            <div class="flex flex-wrap gap-4 items-center justify-center">
                <div>
                    <label for="locationFilter" class="block text-sm font-medium text-stone-700 mb-1">開催地:</label>
                    <select id="locationFilter" class="filter-select mt-1 block w-full pl-3 pr-10 py-2 text-base border-stone-300 focus:outline-none focus:ring-sky-500 focus:border-sky-500 sm:text-sm rounded-md shadow-sm">
                        <option value="all">すべて</option>
                        <option value="東京">東京</option>
                        <option value="大阪">大阪</option>
                        <option value="福岡">福岡</option>
                        <option value="広島">広島</option>
                        <option value="名古屋">名古屋</option>
                        <option value="京都">京都</option>
                        <option value="オンライン">オンライン</option>
                        <option value="全国各地">全国各地</option>
                    </select>
                </div>
                <div>
                    <label for="typeFilter" class="block text-sm font-medium text-stone-700 mb-1">種類:</label>
                    <select id="typeFilter" class="filter-select mt-1 block w-full pl-3 pr-10 py-2 text-base border-stone-300 focus:outline-none focus:ring-sky-500 focus:border-sky-500 sm:text-sm rounded-md shadow-sm">
                        <option value="all">すべて</option>
                        <option value="セミナー">セミナー</option>
                        <option value="USERFEST">USERFEST</option>
                        <option value="キャンペーン">キャンペーン</option>
                        <option value="体験会">体験会</option>
                        <option value="試験">試験</option>
                        <option value="MeetUp">MeetUp</option>
                        <option value="勉強会">勉強会</option>
                    </select>
                </div>
                <div>
                    <label for="monthFilter" class="block text-sm font-medium text-stone-700 mb-1">開催月:</label>
                    <select id="monthFilter" class="filter-select mt-1 block w-full pl-3 pr-10 py-2 text-base border-stone-300 focus:outline-none focus:ring-sky-500 focus:border-sky-500 sm:text-sm rounded-md shadow-sm">
                        <option value="all">すべて</option>
                    </select>
                </div>
                <button id="resetFilters" class="mt-4 sm:mt-0 px-4 py-2 bg-sky-600 text-white rounded-md hover:bg-sky-700 focus:outline-none focus:ring-2 focus:ring-sky-500 focus:ring-offset-2">
                    フィルター解除
                </button>
            </div>
        </div>

        <div id="eventList" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            </div>
        <div id="noResults" class="hidden text-center text-stone-500 mt-8">
            <p class="text-xl">該当するイベントは見つかりませんでした。</p>
            <p>フィルター条件を変更してお試しください。</p>
        </div>

    </div>

    <script>
        const eventsData = [
            // 2025年4月のイベント
            {
                id: 35,
                title: "【WEB】BIMセミナー (4/17)",
                date: "2025年4月17日（木）",
                time: "14:00～16:30",
                location: "オンライン",
                venue: "オンライン",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するオンラインセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 36,
                title: "【東京】BIMセミナー (4/23)",
                date: "2025年4月23日（水）",
                time: "14:00～16:30",
                location: "東京",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 37,
                title: "【大阪】BIMセミナー (4/24)",
                date: "2025年4月24日（木）",
                time: "14:00～16:30",
                location: "大阪",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 38,
                title: "【名古屋】BIMセミナー (4/15)",
                date: "2025年4月15日（火）",
                time: "14:00～16:30",
                location: "名古屋",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 39,
                title: "【福岡】BIMセミナー (4/10)",
                date: "2025年4月10日（木）",
                time: "14:00～16:30",
                location: "福岡",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 40,
                title: "「はじめてのArchicad」無料体験セミナー (オンライン 4/25)",
                date: "2025年4月25日（金）",
                time: "10:00 - 12:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "ArchicadでBIMモデル（RC造）を制作・操作体験",
                link: "https://ken-it.world/supporters/2024/07/7-9%E3%80%8125%E3%80%8130%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%EF%BD%A2%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%EF%BD%A3%E7%84%A1%E6%96%99%E4%BD%93%E9%A8%93%E3%82%BB.html",
                type: "体験会"
            },
            // 2025年5月のイベント
            {
                id: 19,
                title: "建設DXソリューションデモデー2025",
                date: "2025年5月16日(金)",
                time: "10:00～17:00",
                location: "東京",
                venue: "株式会社大塚商会 本社ビル",
                organizer: "大塚商会",
                theme: "AI・業務自動化、CAD（設計支援ツール）など建設DX全般",
                link: "https://www.otsuka-shokai.co.jp/event/region/25/0516cadk/",
                type: "セミナー"
            },
            {
                id: 20,
                title: "ADS-BT for ARCHICAD オンラインセミナー",
                date: "2025年5月16日(金)",
                time: "13:30～14:30",
                location: "オンライン",
                venue: "オンライン",
                organizer: "生活産業研究所",
                theme: "Archicad25対応版、斜線逆日影計算、日影計算、天空率計算など",
                link: "https://www.epcot.co.jp/2025/04/btac250516.php",
                type: "セミナー"
            },
            {
                id: 21,
                title: "「はじめてのArchicad」オンライン体験セミナー (木造編 5/27)",
                date: "2025年5月27日（火）",
                time: "14:00 - 16:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "Archicad基本操作～BIMモデル作成体験 (木造編)",
                link: "https://ken-it.world/supporters/2025/05/5-27%E3%80%816-12%E3%80%816-24%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%E3%80%8C%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%E3%80%8D%E6%9C%A8%E9%80%A0%E7%B7%A8.html",
                type: "体験会"
            },
            {
                id: 22,
                title: "「はじめてのArchicad」オンライン体験セミナー (RC造編 5/27)",
                date: "2025年5月27日（火）",
                time: "10:00 - 12:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "Archicad基本操作～BIMモデル作成体験 (RC造編)",
                link: "https://ken-it.world/supporters/2025/05/5-27%E3%80%816-12%E3%80%816-24%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%EF%BD%A2%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%EF%BD%A3-rc%E9%80%A0%E7%B7%A8.html",
                type: "体験会"
            },
            {
                id: 23,
                title: "Archicad 体験セミナー（東京会場） - ユニマットリック",
                date: "2025年5月22日(木)",
                time: "14:30～16:30",
                location: "東京",
                venue: "株式会社ユニマットリック東京営業所",
                organizer: "ユニマットリック",
                theme: "Archicad無料体験、植栽のワンポイントテクニック",
                link: "https://rik-service.net/form/seminar/course-groups/2295",
                type: "体験会"
            },
            {
                id: 24,
                title: "BIM Classes体験セミナー",
                date: "2025年5月26日(月)",
                time: "未定",
                location: "オンライン",
                venue: "オンライン",
                organizer: "グラフィソフトジャパン",
                theme: "Archicadのトレーニングプログラム体験",
                link: "https://www.archifuture-web.jp/finder/3115.html",
                type: "体験会"
            },
            // 2025年6月のイベント (既存のイベント)
            {
                id: 1,
                title: "【東京】全国ロードショー",
                date: "2025年6月3日（火）",
                time: "14:00 - 16:30 (受付開始 13:30)",
                location: "東京",
                venue: "コモレ四谷 タワーコンファレンス",
                organizer: "グラフィソフトジャパン",
                theme: "OPENBIMで広がる未来への展望 ～設計・原価連携DX運用～",
                capacity: "100名",
                link: "https://graphisoft.com/jp/open-events/roadshow/tokyo-25q2",
                type: "セミナー"
            },
            {
                id: 2,
                title: "【福岡】全国ロードショー",
                date: "2025年6月4日（水）",
                time: "14:00 - 16:30 (受付開始 13:30)",
                location: "福岡",
                venue: "福岡リファレンス大博多ビル",
                organizer: "グラフィソフトジャパン",
                theme: "Archicadデフォルト機能での施工図作成",
                capacity: "60名",
                link: "https://graphisoft.com/jp/open-events/roadshow", 
                type: "セミナー"
            },
            {
                id: 3,
                title: "【大阪】全国ロードショー",
                date: "2025年6月5日（木）",
                time: "14:00 - 16:30 (受付開始 13:30)",
                location: "大阪",
                venue: "グランフロント大阪",
                organizer: "グラフィソフトジャパン",
                theme: "ふだんづかいの設計ツール",
                capacity: "60名",
                link: "https://graphisoft.com/jp/open-events/roadshow",
                type: "セミナー"
            },
            {
                id: 4,
                title: "USERFEST2025",
                date: "2025年6月20日（金）",
                time: "12:00 ～ 19:00 (開場 11:00)",
                location: "広島",
                venue: "詳細会場は後日発表 (懇親会 19:00～21:30)",
                organizer: "グラフィソフトジャパン",
                theme: "ArchiCADユーザーの祭典。前日6/19には広島県建築士会主催「Archi Session Vol.2」も開催。",
                link: "https://community.graphisoft.com/t5/USERFEST2025-%E3%82%A4%E3%83%B3%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3/USERFEST2025-%E9%96%8B%E5%82%AC%E6%97%A5%E3%81%A8%E4%BC%9A%E5%A0%B4-%E7%99%BA%E8%A1%A8/td-p/657626",
                type: "USERFEST"
            },
            {
                id: 5,
                title: "期間限定サブスクリプション移行プログラム応援キャンペーン",
                date: "2025年6月末～12月末",
                time: "期間中",
                location: "オンライン", 
                venue: "オンライン",
                organizer: "グラフィソフトジャパン",
                theme: "VIPservice契約またはGraphisoft Forward契約期間満了を迎えるお客様対象。",
                link: "https://graphisoft.com/jp/campaigns",
                type: "キャンペーン"
            },
            {
                id: 6,
                title: "【福岡】BIMセミナー",
                date: "2025年6月5日（木）",
                time: "14:00～16:30",
                location: "福岡",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 7,
                title: "【名古屋】BIMセミナー",
                date: "2025年6月10日（火）",
                time: "14:00～16:30",
                location: "名古屋",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 8,
                title: "【東京】BIMセミナー (6/11)",
                date: "2025年6月11日（水）",
                time: "14:00～16:30",
                location: "東京",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 9,
                title: "【東京】BIMセミナー (6/25)",
                date: "2025年6月25日（水）",
                time: "14:00～16:30",
                location: "東京",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 10,
                title: "【大阪】BIMセミナー",
                date: "2025年6月19日（木）",
                time: "14:00～16:30",
                location: "大阪",
                venue: "未定 (詳細はリンク先)",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 11,
                title: "【WEB】BIMセミナー",
                date: "2025年6月26日（木）",
                time: "14:00～16:30",
                location: "オンライン",
                venue: "オンライン",
                organizer: "株式会社エービーケーエスエス",
                theme: "BIM活用に関するオンラインセミナー",
                link: "https://www.abkss.jp/seminar/archicad",
                type: "セミナー"
            },
            {
                id: 12,
                title: "「はじめてのArchicad」オンライン体験セミナー (木造編 6/12)",
                date: "2025年6月12日（木）",
                time: "14:00 - 16:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "Archicad基本操作～BIMモデル作成体験 (木造編)",
                link: "https://ken-it.world/supporters/2025/05/5-27%E3%80%816-12%E3%80%816-24%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%E3%80%8C%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%E3%80%8D%E6%9C%A8%E9%80%A0%E7%B7%A8.html",
                type: "体験会"
            },
            {
                id: 13,
                title: "「はじめてのArchicad」オンライン体験セミナー (木造編 6/24)",
                date: "2025年6月24日（火）",
                time: "10:00 - 12:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "Archicad基本操作～BIMモデル作成体験 (木造編)",
                link: "https://ken-it.world/supporters/2025/05/5-27%E3%80%816-12%E3%80%816-24%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%E3%80%8C%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%E3%80%8D%E6%9C%A8%E9%80%A0%E7%B7%A8.html",
                type: "体験会"
            },
            {
                id: 14,
                title: "「はじめてのArchicad」オンライン体験セミナー (RC造編 6/12)",
                date: "2025年6月12日（木）",
                time: "10:00 - 12:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "Archicad基本操作～BIMモデル作成体験 (RC造編)",
                link: "https://ken-it.world/supporters/2025/05/5-27%E3%80%816-12%E3%80%816-24%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%EF%BD%A2%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%EF%BD%A3-rc%E9%80%A0%E7%B7%A8.html",
                type: "体験会"
            },
            {
                id: 15,
                title: "「はじめてのArchicad」オンライン体験セミナー (RC造編 6/24)",
                date: "2025年6月24日（火）",
                time: "14:00 - 16:00",
                location: "オンライン",
                venue: "オンライン (Teams, Archicadデモモード)",
                organizer: "グラフィソフトジャパン (建設ITワールド経由)",
                theme: "Archicad基本操作～BIMモデル作成体験 (RC造編)",
                link: "https://ken-it.world/supporters/2025/05/5-27%E3%80%816-12%E3%80%816-24%E3%82%AA%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%80%81%EF%BD%A2%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AEarchicad%EF%BD%A3-rc%E9%80%A0%E7%B7%A8.html",
                type: "体験会"
            },
            {
                id: 16,
                title: "BIM利用技術者試験 1級・準1級（前期）",
                date: "2025年6月29日（日）",
                time: "終日",
                location: "全国各地", 
                venue: "札幌・仙台・東京・名古屋・大阪・広島・福岡（2024年度実績）",
                organizer: "コンピュータ教育振興協会",
                theme: "BIM利用技術者試験。申込期間: 2025年4月21日～5月22日。",
                link: "https://archi.fukuicompu.co.jp/event/detail.php?rec_id=17499",
                type: "試験"
            },
            {
                id: 17,
                title: "建築ビジュアライゼーションMeetUp第8弾",
                date: "2025年6月25日（水）",
                time: "未定 (詳細はリンク先)",
                location: "東京",
                venue: "東京・虎ノ門",
                organizer: "株式会社Too",
                theme: "建築設計業界向け交流イベント。新たな手法・表現に関心のある方も。",
                link: "https://prtimes.jp/main/html/rd/p/000000263.000004669.html",
                type: "MeetUp"
            },
            {
                id: 18,
                title: "国産BIM GLOOBE Architect 操作体験会【東京会場】",
                date: "2025年6月13日（金）",
                time: "10:00～12:00",
                location: "東京",
                venue: "福井コンピュータアーキテクト（株）関東営業所",
                organizer: "福井コンピュータアーキテクト",
                theme: "GLOOBE Architectの基本操作体験。定員5名。",
                capacity: "5名",
                link: "https://archi.fukuicompu.co.jp/event/detail.php?rec_id=17554",
                type: "体験会"
            }
        ];

        const eventListContainer = document.getElementById('eventList');
        const locationFilter = document.getElementById('locationFilter');
        const typeFilter = document.getElementById('typeFilter');
        const monthFilter = document.getElementById('monthFilter');
        const resetFiltersButton = document.getElementById('resetFilters');
        const noResultsDiv = document.getElementById('noResults');

        function renderEvents(eventsToRender) {
            eventListContainer.innerHTML = '';
            if (eventsToRender.length === 0) {
                noResultsDiv.classList.remove('hidden');
            } else {
                noResultsDiv.classList.add('hidden');
            }

            eventsToRender.sort((a, b) => {
                const dateA = new Date(a.date.replace(/（.*?）/g, '').replace(/年|月/g, '-').replace(/日/g, ''));
                const dateB = new Date(b.date.replace(/（.*?）/g, '').replace(/年|月/g, '-').replace(/日/g, ''));
                if (isNaN(dateA.getTime()) || isNaN(dateB.getTime())) return 0;
                return dateA - dateB;
            });
            
            eventsToRender.forEach(event => {
                const card = `
                    <div class="event-card bg-white rounded-xl shadow-lg overflow-hidden p-6 flex flex-col justify-between">
                        <div>
                            <h2 class="text-xl font-semibold text-sky-700 mb-2">${event.title}</h2>
                            <p class="text-sm text-stone-600 mb-1"><span class="font-medium">📅 日時:</span> ${event.date} ${event.time}</p>
                            <p class="text-sm text-stone-600 mb-1"><span class="font-medium">📍 場所:</span> ${event.location} (${event.venue})</p>
                            <p class="text-sm text-stone-600 mb-1"><span class="font-medium">🏢 主催:</span> ${event.organizer}</p>
                            ${event.capacity ? `<p class="text-sm text-stone-600 mb-1"><span class="font-medium">👥 定員:</span> ${event.capacity}</p>` : ''}
                            <p class="text-sm text-stone-600 mt-2 mb-4"><span class="font-medium">📝 概要:</span> ${event.theme}</p>
                        </div>
                        <a href="${event.link}" target="_blank" rel="noopener noreferrer" class="mt-auto inline-block w-full text-center bg-sky-600 text-white px-4 py-2 rounded-md hover:bg-sky-700 transition duration-150 ease-in-out text-sm font-medium">
                            詳細・申込はこちら
                        </a>
                    </div>
                `;
                eventListContainer.innerHTML += card;
            });
        }

        function applyFilters() {
            const selectedLocation = locationFilter.value;
            const selectedType = typeFilter.value;
            const selectedMonth = monthFilter.value;

            const filteredEvents = eventsData.filter(event => {
                const locationMatch = selectedLocation === 'all' || event.location === selectedLocation || (selectedLocation === '全国各地' && event.location === '全国各地');
                 if (selectedLocation === '全国各地' && event.title === "BIM利用技術者試験 1級・準1級（前期）") {
                    return true;
                 }
                const typeMatch = selectedType === 'all' || event.type === selectedType;
                
                let eventMonthYear = '';
                try {
                    const eventDate = new Date(event.date.replace(/（.*?）/g, '').replace(/年|月/g, '-').replace(/日/g, ''));
                    if (!isNaN(eventDate.getTime())) {
                        eventMonthYear = eventDate.getFullYear() + '年' + (eventDate.getMonth() + 1) + '月';
                    }
                } catch (e) {
                    console.error("Error parsing date for event:", event.title, e);
                }
                const monthMatch = selectedMonth === 'all' || eventMonthYear === selectedMonth;

                return locationMatch && typeMatch && monthMatch;
            });
            renderEvents(filteredEvents);
        }
        
        function populateFilterOptions() {
            const locations = new Set();
            const types = new Set();
            const monthsToDisplay = new Set(); // Months that should be displayed in filter

            // Get current date
            const today = new Date();
            let currentYear = today.getFullYear();
            let currentMonth = today.getMonth(); // 0-indexed month

            // Add previous month, current month, and next month to monthsToDisplay
            for (let i = -1; i <= 1; i++) {
                let d = new Date(currentYear, currentMonth + i, 1);
                monthsToDisplay.add(d.getFullYear() + '年' + (d.getMonth() + 1) + '月');
            }

            eventsData.forEach(event => {
                locations.add(event.location);
                types.add(event.type);
                
                let eventMonthYear = '';
                try {
                    const eventDate = new Date(event.date.replace(/（.*?）/g, '').replace(/年|月/g, '-').replace(/日/g, ''));
                    if (!isNaN(eventDate.getTime())) {
                        eventMonthYear = eventDate.getFullYear() + '年' + (eventDate.getMonth() + 1) + '月';
                    }
                } catch (e) {
                    console.error("Error parsing date for event:", event.title, e);
                }
            });

            // Sort months chronologically
            const sortedMonths = Array.from(monthsToDisplay).sort((a, b) => {
                const dateA = new Date(a.replace(/年|月/g, '-').slice(0, -1));
                const dateB = new Date(b.replace(/年|月/g, '-').slice(0, -1));
                return dateA - dateB;
            });

            // Add "すべて" option first
            const allMonthOption = document.createElement('option');
            allMonthOption.value = 'all';
            allMonthOption.textContent = 'すべて';
            monthFilter.appendChild(allMonthOption);

            sortedMonths.forEach(month => {
                const option = document.createElement('option');
                option.value = month;
                option.textContent = month;
                monthFilter.appendChild(option);
            });

            locations.forEach(location => {
                if (!Array.from(locationFilter.options).some(opt => opt.value === location)) {
                    const option = document.createElement('option');
                    option.value = location;
                    option.textContent = location;
                    locationFilter.appendChild(option);
                }
            });

            types.forEach(type => {
                 if (!Array.from(typeFilter.options).some(opt => opt.value === type)) {
                    const option = document.createElement('option');
                    option.value = type;
                    option.textContent = type;
                    typeFilter.appendChild(option);
                 }
            });
        }


        locationFilter.addEventListener('change', applyFilters);
        typeFilter.addEventListener('change', applyFilters);
        monthFilter.addEventListener('change', applyFilters);
        resetFiltersButton.addEventListener('click', () => {
            locationFilter.value = 'all';
            typeFilter.value = 'all';
            monthFilter.value = 'all';
            applyFilters();
        });

        populateFilterOptions();
        applyFilters();
    </script>
</body>
</html>
