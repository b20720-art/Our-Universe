<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>우리가 왜 싸웠을까?</title>
    <style>
        /* 기본 스타일 설정 (모바일 최적화 및 심플 UI) */
        :root {
            --bg-color: #f8f9fa;
            --card-color: #ffffff;
            --text-color: #212529;
            --primary-color: #4f46e5;
            --border-color: #e5e7eb;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            line-height: 1.6;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 480px; /* 모바일 화면 최적화 */
            background: var(--card-color);
            border-radius: 16px;
            padding: 24px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }

        .hidden {
            display: none !important;
        }

        /* 메인 화면 */
        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 1.5rem;
            color: var(--primary-color);
            margin-bottom: 8px;
        }

        .header p {
            font-size: 0.95rem;
            color: #6b7280;
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-group label {
            display: block;
            font-size: 0.9rem;
            font-weight: 600;
            margin-bottom: 8px;
        }

        select {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            font-size: 1rem;
            background-color: #fff;
            outline: none;
        }

        /* 버튼 스타일 */
        .btn {
            width: 100%;
            padding: 14px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }

        .btn:hover {
            background-color: #4338ca;
        }

        /* 질문 화면 */
        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: var(--border-color);
            border-radius: 3px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background-color: var(--primary-color);
            width: 0%;
            transition: width 0.3s;
        }

        .question-text {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 24px;
            word-break: keep-all;
        }

        .options-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option-btn {
            width: 100%;
            padding: 14px;
            text-align: left;
            background: #fff;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.2s;
        }

        .option-btn:hover {
            border-color: var(--primary-color);
            background-color: #f5f3ff;
        }

        /* 결과 화면 (카드형) */
        .result-card {
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 18px;
            margin-bottom: 16px;
            background-color: #fafafa;
        }

        .result-title {
            font-weight: 600;
            font-size: 1rem;
            margin-bottom: 6px;
            color: var(--primary-color);
        }

        .result-score {
            font-size: 2rem;
            font-weight: 700;
            text-align: center;
            color: var(--primary-color);
            margin: 15px 0;
        }

        .section-title {
            font-size: 1rem;
            font-weight: 600;
            margin-top: 20px;
            margin-bottom: 8px;
            border-left: 4px solid var(--primary-color);
            padding-left: 8px;
        }

        ul {
            padding-left: 20px;
            font-size: 0.95rem;
            color: #4b5563;
        }

        li {
            margin-bottom: 6px;
        }
    </style>
</head>
<body>

<div class="container">
    <div id="home-screen">
        <div class="header">
            <h1>우리가 싸운 건 누구의 잘못일까?</h1>
            <p>서로의 성향 차이를 분석하고 갈등을 해결해 보세요.</p>
        </div>
        
        <div class="input-group">
            <label for="my-name">내 이름 (또는 별명)</label>
            <select id="my-name">
                <option value="나">나</option>
            </select>
        </div>

        <div class="input-group">
            <label for="partner-mbti">상대방의 MBTI</label>
            <select id="partner-mbti">
                <option value="T">T형 (사고형 - 논리적, 객관적)</option>
                <option value="F">F형 (감정형 - 공감적, 관계중심적)</option>
            </select>
        </div>

        <button class="btn" onclick="startTest()">갈등 상황 테스트 시작</button>
    </div>

    <div id="question-screen" class="hidden">
        <div class="progress-bar">
            <div id="progress" class="progress"></div>
        </div>
        <div id="question-text" class="question-text">질문 내용이 들어갑니다.</div>
        <div id="options" class="options-list">
            </div>
    </div>

    <div id="result-screen" class="hidden">
        <div class="header">
            <h1>궁합 및 갈등 분석 결과</h1>
            <p>우리가 싸웠던 진짜 이유를 알아봅시다.</p>
        </div>

        <div class="result-card">
            <div class="result-title">우리의 소통 궁합 점수</div>
            <div id="result-score" class="result-score">70점</div>
        </div>

        <div class="section-title">⚠️ 주요 갈등 포인트</div>
        <p id="conflict-point" style="font-size: 0.95rem; color: #4b5563; margin-bottom: 12px;"></p>

        <div class="section-title">💡 상황별 행동 가이드</div>
        <ul id="action-guide">
            </ul>

        <div class="section-title">💬 추천 대화법 및 팁</div>
        <ul id="tip-list">
            </ul>

        <button class="btn" style="margin-top: 24px;" onclick="restart()">다시 테스트하기</button>
    </div>
</div>

<script>
    // 분석가(Analyst) 영역의 상황형 질문 반영
    const questions = [
        {
            text: "1. 나와 상대방이 의견 차이로 다투게 되었을 때, 나의 행동에 가장 가까운 것은?",
            options: [
                { text: "이유를 먼저 묻고 논리적으로 잘잘못을 따져본다.", type: "T" },
                { text: "상대방이 서운해하는 감정을 먼저 헤아리려 노력한다.", type: "F" },
                { text: "당장 불편한 상황을 피하기 위해 대화를 참고 넘긴다.", type: "P" }
            ]
        },
        {
            text: "2. 상대방이 힘든 일을 털어놓았을 때, 내가 주로 건네는 말은?",
            options: [
                { text: "그 상황을 해결할 수 있는 현실적인 조언과 대안을 준다.", type: "T" },
                { text: "마음이 많이 상했겠다고 말하며 적극적으로 위로하고 공감한다.", type: "F" },
                { text: "크게 신경 쓰지 마라며 분위기를 바꾸려고 노력한다.", type: "P" }
            ]
        },
        {
            text: "3. 약속 시간에 상대방이 늦었을 때 내가 느끼는 생각은?",
            options: [
                { text: "늦은 정당한 이유가 무엇인지 객관적인 사실이 중요하다.", type: "T" },
                { text: "기다리는 나에 대한 성의나 미안해하는 태도가 더 중요하다.", type: "F" },
                { text: "그럴 수도 있다고 생각하며 유연하게 넘어간다.", type: "P" }
            ]
        }
    ];

    // 결과 데이터 정의 (아키텍트 설계 기반 출력물 구조)
    const resultDatabase = {
        "T-T": {
            score: "85점",
            conflict: "서로 논리만 내세우다 감정이 상할 수 있으며, 잘잘못을 가리려다 대화가 법정 공방처럼 변할 수 있습니다.",
            guides: ["이성적인 대화가 통하는 사이니 팩트를 짚되 비난조를 피하세요.", "상대방의 의견에 숨겨진 의도나 논리를 경청하세요."],
            tips: ["'네 말이 맞는데, 내 생각은 이래'와 같이 상대의 논리를 먼저 인정해 주기.", "서로 지치지 않도록 타협점을 명확히 구체화하기."]
        },
        "T-F": {
            score: "50점",
            conflict: "T는 문제를 해결하려 하고 F는 마음을 위로받고 싶어 합니다. T의 직설적인 조언이 F에게 상처를 주거나, F의 감정적 호소가 T를 지치게 만듭니다.",
            guides: ["T는 조언하기 전에 '많이 속상했겠다'라는 공감을 먼저 표현하세요.", "F는 서운한 감정만 앞세우기보다 원하는 해결책을 명확히 말해보세요."],
            tips: ["T에게: '공감 5분 후 조언하기' 규칙을 써보세요.", "F에게: 상대의 조언이 나를 공격하는 게 아니라 도우려는 마음임을 기억하세요."]
        },
        "F-T": {
            score: "50점",
            conflict: "내가 서운함을 표현할 때 상대방이 지나치게 이성적이거나 냉정하게 상황을 분석하여# Our-Universe
