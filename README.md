<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>오늘의 급식</title>
  <style>
    body { font-family: sans-serif; margin: 0; background: #f5f5f5; }
    
    nav {
      background: #4CAF50; color: white;
      display: flex; justify-content: space-between;
      align-items: center; padding: 12px 20px;
    }
    nav .links a {
      color: white; text-decoration: none; margin-left: 15px;
    }
    nav .links a:hover {
      text-decoration: underline;
    }

    #content { padding: 20px; }

    .meal-container {
      display: flex;
      justify-content: space-between;
      gap: 20px;
    }
    .meal-box {
      flex: 1;
      background: #fff;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      min-height: 200px;
    }
    .meal-box h2 {
      text-align: center;
    }

    @media (max-width: 768px) {
      .meal-container {
        flex-direction: column;
      }
    }
  </style>
</head>
<body>

  <!-- 상단 메뉴 -->
  <nav>
    <div><strong>학교 급식 알리미</strong></div>
    <div class="links">
      <a href="google.form.com" target="_blank">급식 설문조사 참여하기</a>
    </div>
  </nav>

  <!-- 본문 영역 -->
  <div id="content">
    <h1>영산고등학교 오늘의 급식</h1>
    <div class="meal-container">
      <div class="meal-box" id="breakfast"><h2>조식</h2></div>
      <div class="meal-box" id="lunch"><h2>중식</h2></div>
      <div class="meal-box" id="dinner"><h2>석식</h2></div>
    </div>
  </div>

  <!-- 급식 정보 불러오기 -->
  <script>
    const apiKey = 'c051da8fcae04fa2b4f58d0b510df348';
    const eduCode = 'Q10';
    const schoolCode = '8490325';
    const today = new Date().toISOString().slice(0, 10).replace(/-/g, '');

    fetch(`https://open.neis.go.kr/hub/mealServiceDietInfo?KEY=${apiKey}&Type=json&ATPT_OFCDC_SC_CODE=${eduCode}&SD_SCHUL_CODE=${schoolCode}&MLSV_YMD=${today}`)
      .then(res => res.json())
      .then(data => {
        const meals = data.mealServiceDietInfo[1].row;
        meals.forEach(m => {
          const code = m.MMEAL_SC_CODE;
          const dish = m.DDISH_NM.replace(/<br\/>/g, '<br>');
          if (code === '1') document.getElementById('breakfast').innerHTML += `<p>${dish}</p>`;
          if (code === '2') document.getElementById('lunch').innerHTML += `<p>${dish}</p>`;
          if (code === '3') document.getElementById('dinner').innerHTML += `<p>${dish}</p>`;
        });
      })
      .catch(() => {
        ['breakfast', 'lunch', 'dinner'].forEach(id => {
          document.getElementById(id).innerHTML += `<p>불러올 수 없습니다.</p>`;
        });
      });
  </script>

</body>
</html>
