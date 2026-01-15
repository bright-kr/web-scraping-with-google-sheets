# Google Sheets로 Web 데이터 추출하기

[![Bright Data Promo](https://github.com/bright-kr/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.co.kr/)

코딩 경험 없이도 웹사이트에서 유용한 데이터를 추출할 수 있도록, Google Sheets의 [IMPORTXML](https://support.google.com/docs/answer/3093342?hl=en) 및 [IMPORTHTML](https://support.google.com/docs/answer/3093339?hl=en) 함수를 활용하는 방법을 알아보겠습니다.

- [Google Sheets를 Web스크레이핑에 활용하는 이점](#benefits-of-google-sheets-for-web-scraping)
- [첫 번째 스크레이핑 시트 만들기](#creating-your-first-scraping-sheet)
- [필수 Google Sheets 스크레이핑 함수](#essential-google-sheets-scraping-functions)
  - [IMPORTXML 사용하기](#using-importxml)
  - [IMPORTHTML 활용하기](#working-with-importhtml)
- [단계별 데이터 추출 가이드](#step-by-step-data-extraction-guide)
- [제한 사항 및 고급 시나리오](#limitations-and-advanced-scenarios)
- [자동 데이터 업데이트 설정](#setting-up-automatic-data-updates)
- [스크레이핑 프로세스 최적화](#optimizing-your-scraping-process)
- [다음 단계](#next-steps)

## Google Sheets를 Web Scraping에 활용하는 이점

Google Sheets는 프로그래밍 지식 없이도 데이터 추출을 수행할 수 있는 의외로 강력한 솔루션을 제공합니다. 웹사이트에서 구조화된 데이터와 표 형식 데이터를 수집하는 데 강점이 있으며, 수집한 내용을 즉시 분석하거나 시각화할 수 있습니다. 따라서 다음과 같은 다양한 사용 사례에 적합합니다.

* 이커머스 플랫폼에서 상품 가격 모니터링
* 온라인 디렉터리에서 연락처 목록 구축
* 소셜 채널 전반의 참여 지표 추적
* 마케팅 분석을 위한 공개 여론 수집

수집한 데이터는 CSV 또는 기타 형식으로 쉽게 내보내 기존 시스템과 통합할 수 있습니다.

## 첫 번째 Scraping Sheet 만들기

시작하려면 [https://sheets.google.com](https://sheets.google.com/) 로 이동한 뒤 **+** 아이콘을 선택하여 새 스프레드시트를 시작합니다.

![Google Sheets new document creation](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-24-1024x241.png)

Web스크레이핑 기법 학습을 위해 특별히 설계된 데모 사이트인 [**Books to Scrape**](https://books.toscrape.com/catalogue/category/books/default_15/index.html)를 사용해 보겠습니다.

## 필수 Google Sheets Scraping Functions

Google Sheets에는 스프레드시트 내에서 직접 데이터 추출을 가능하게 하는 여러 강력한 [수식](https://support.google.com/docs/table/25273?hl=en)이 포함되어 있습니다. 여기서는 Web스크레이핑에 가장 유용한 두 가지 함수를 살펴보겠습니다.

### IMPORTXML 사용하기

[`IMPORTXML`](https://support.google.com/docs/answer/3093342?sjid=2557491900941403739-NC) 함수는 XPath 셀렉터를 사용하여 구조화된 데이터를 스프레드시트로 가져옵니다. 이 함수는 XML, HTML, CSV, TSV 형식에서 동작하며, 다음 구조를 따릅니다.

```
=IMPORTXML(url, xpath_query)
```

이 함수는 [XPath](https://developer.mozilla.org/en-US/docs/Web/XPath)를 사용해 특정 요소를 타겟팅하여 어떤 웹 URL에서든 데이터를 가져옵니다. 예를 들어 데모 사이트에서 메인 제목을 추출하려면 다음 수식을 입력합니다.

```
=IMPORTXML("https://books.toscrape.com/catalogue/category/books/default_15/index.html", "//h1")
```

이 함수를 처음 사용할 때 Google Sheets는 외부 사이트에 연결하기 위한 권한을 요청합니다.

![Google Sheets access permission dialog](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-25-1024x272.png)

**Allow access**를 클릭하면 셀에 대상 페이지의 H1 제목 콘텐츠인 "Default"가 표시됩니다.

### IMPORTHTML 활용하기

[`IMPORTHTML`](https://support.google.com/docs/answer/3093339?sjid=2557491900941403739-NC) 함수는 웹 페이지에서 표와 목록을 추출하는 데 특화되어 있으며, 다음 형식을 사용합니다.

```
=IMPORTHTML(url, query, index)
```

이 함수는 `query` 파라メータ(“table” 또는 “list”)와 `index` 번호(1부터 시작)를 기반으로 어떤 표 또는 목록을 가져올지 지정하여 데이터를 추출합니다. 예를 들어, 예시 사이트에서 도서 목록을 추출하려면 다음과 같이 입력합니다.

```
=IMPORTHTML("https://books.toscrape.com/catalogue/category/books/default_15/index.html", "list", 2)
```

이 수식은 스프레드시트에 전체 도서 목록을 채웁니다.

![Imported book list in Google Sheets](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-26-1024x557.png)

## 단계별 데이터 추출 가이드

이제 기본을 이해했으므로, 더 구조화된 추출 프로세스를 만들어 보겠습니다. IMPORTXML을 사용해 Books to Scrape 웹사이트에서 도서 제목, 가격, 평점을 캡처하겠습니다.

먼저 스프레드시트에 적절한 열 헤더를 설정합니다.

![Google Sheets with column headers](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-27-1024x425.png)

도서 제목에 대한 올바른 XPath를 찾기 위해 브라우저의 개발자 도구를 사용합니다.

1. 첫 번째 도서 제목을 마우스 오른쪽 버튼으로 클릭합니다.
2. **Inspect**를 선택합니다.
3. 하이라이트된 HTML 요소를 마우스 오른쪽 버튼으로 클릭합니다.
4. **Copy > XPath**를 선택합니다.

![Finding XPath using browser developer tools](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-28-1024x498.png)

단일 도서 제목에 대한 원시 XPath는 다음과 같이 보일 수 있습니다.

```
//*[@id="default"]/div/div/div/div/section/div[2]/ol/li[1]/article/h3/a
```

모든 도서 제목을 추출하려면 이 XPath를 수정해야 합니다.

* 모든 목록 항목을 타겟팅하기 위해 `li[1]`을 `li`로 바꿉니다.
* 전체 제목 속성을 캡처하기 위해 `a`를 `a/@title`로 변경합니다.
* XPath 내부의 큰따옴표를 작은따옴표로 변환합니다.

셀 A2에 다음 최적화된 수식을 입력합니다.

```
=IMPORTXML("https://books.toscrape.com/catalogue/category/books/default_15/index.html", "//*[@id='default']/div/div/div/div/section/div[2]/ol/li/article/h3/a/@title")
```

시트에 모든 도서 제목이 채워집니다.

![Google Sheets showing imported book titles](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-29-1024x557.png)

다음으로 셀 B2에 가격 데이터 수식을 추가합니다.

```
=IMPORTXML("https://books.toscrape.com/catalogue/category/books/default_15/index.html", "//*[@id='default']/div/div/div/div/section/div[2]/ol/li/article/div[2]/p[1]")
```

마지막으로 셀 C2에서 평점을 캡처합니다.

```
=IMPORTXML("https://books.toscrape.com/", "//*[@id='default']/div/div/div/div/section/div[2]/ol/li/article/p/@class")
```

완성된 스프레드시트에는 세 가지 데이터 포인트가 모두 표시됩니다.

![Complete spreadsheet with book data](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-30-1024x557.png)

평점은 `star-rating Three` 또는 `star-rating Four`처럼 표시됩니다. 안타깝게도 Google Sheets는 [XPath 2.0](https://www.w3.org/TR/xpath20/)을 지원하지 않으므로, 수식에서 이 데이터를 직접 변환할 수 없습니다.

## 제한 사항 및 고급 시나리오

Google Sheets는 기본적인 스크레이핑에는 잘 작동하지만 다음과 같은 경우에는 중요한 제한이 있습니다.

**동적 콘텐츠**: 웹사이트가 초기 페이지 렌더링 이후 JavaScript로 데이터를 로드하는 경우, Google Sheets 수식은 정적 HTML만 처리하므로 해당 콘텐츠를 캡처하지 못합니다. 동적 사이트의 경우 headless browser를 사용하는 Python 스크립트가 필요합니다.

**페이지네이션**: Google Sheets는 여러 페이지를 자동으로 탐색할 수 없습니다. 각 페이지에 대해 URL과 수식을 수동으로 업데이트해야 하며, 이는 금방 비현실적이 됩니다.

**인터랙티브 요소**: 데이터를 표시하기 전에 클릭, 스크롤 또는 폼 제출이 필요한 웹사이트는 Google Sheets의 기능 범위를 벗어납니다.

이러한 고급 시나리오에서는 프록시, CAPTCHA, user agent 로ーテ이션을 자동으로 처리하는 Bright Data의 포괄적인 스크레이핑 솔루션을 고려해 보시기 바랍니다.

## 자동 데이터 업데이트 설정

가격 추적 또는 모니터링 애플리케이션의 경우, 데이터를 자동으로 새로 고치도록 설정하는 것이 좋습니다.

Google Sheets에서 업데이트 빈도를 구성하려면 다음을 수행합니다.

1. **File > Settings**를 클릭합니다.
2. **Calculation** 탭으로 이동합니다.
3. 원하는 재계산 간격을 설정합니다.

![Google Sheets settings menu](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-31-1024x558.png)

1분 또는 1시간 새로고침 간격 중에서 선택할 수 있습니다.

![Google Sheets recalculation settings](https://github.com/bright-kr/web-scraping-with-google-sheets/blob/main/images/image-32-1024x619.png)

Google Sheets는 이 두 가지 새로고침 옵션으로 제한되지만, Bright Data와 같은 전용 스크레이핑 솔루션은 더 유연한 스케줄링을 제공하며 여러 형식(JSON, CSV, Parquet)으로 데이터를 제공하므로 엔터프라이즈 규모의 데이터 수집에 적합합니다.

## Scraping Process 최적화

스크레이핑 효율을 개선하고 잠재적 이슈를 줄이려면 다음을 고려하시기 바랍니다.

**선별적으로 추출하기**: 필요한 특정 데이터 포인트만 추출하고, 대상 웹사이트에 불필요한 부하를 주지 않도록 합니다.

**지연 적용하기**: 대규모 프로젝트의 경우 요청 사이에 일시 정지를 추가하고, 속도 제한 또는 IP 차단을 유발하지 않도록 비혼잡 시간대에 스케줄링합니다.

**アンチ스크레이핑 대응**: 많은 사이트가 자동화된 접근을 탐지하기 위해 CAPTCHA 챌린지를 사용합니다. 민감한 스크레이핑 작업의 경우 [자동 IP 로ーテ이션을 제공하는 프록시](https://brightdata.co.kr/solutions/rotating-proxies) 사용을 고려하시기 바랍니다.

**법적 요구사항 검토하기**: 스크레이핑 전에 항상 웹사이트의 이용약관과 [`robots.txt`](https://brightdata.co.kr/blog/how-tos/robots-txt-for-web-scraping-guide) 파일을 확인하시기 바랍니다.

## Next Steps

Google Sheets는 특히 구조화된 데이터를 가진 정적 웹사이트에 대해 Web스크레이핑을 시작하기 위한 훌륭한 진입점을 제공합니다.

동적 콘텐츠, 대용량, 또는 정교한 [アンチ스크레이핑 대책](https://brightdata.co.kr/blog/web-data/anti-scraping-techniques)이 필요한 더 복잡한 요구사항의 경우, [Bright Data's Web Scraper API](https://brightdata.co.kr/products/web-scraper)는 프록시, CAPTCHA, 그리고 다양한 출력 형식에 대한 내장 처리를 제공하는 확장 가능한 솔루션을 제공합니다.

지금 무료 체험에 가입하고 데이터 워크플로우 최적화를 시작해 보시기 바랍니다!