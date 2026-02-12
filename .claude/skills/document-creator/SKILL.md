---
name: document-creator
description: PDF, PPTX, XLSX, DOCX 등 문서를 생성하고 편집한다. 프레젠테이션, 보고서, 스프레드시트 제작이 필요할 때 사용한다.
argument-hint: "[문서 유형과 내용]"
user-invocable: true
---

# Document Creator

출처: Anthropic 공식 skills (pdf, pptx, xlsx, docx)

## PDF 생성/편집
```python
# ReportLab로 PDF 생성
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas

c = canvas.Canvas("report.pdf", pagesize=A4)
c.drawString(72, 800, "Title")
c.save()
```

### PDF 작업
- 텍스트/테이블 추출 (pdfplumber, PyMuPDF)
- PDF 병합/분할 (PyPDF)
- 폼 필드 채우기
- 워터마크 추가

## PowerPoint (PPTX)
```python
from pptx import Presentation
from pptx.util import Inches, Pt

prs = Presentation()
slide = prs.slides.add_slide(prs.slide_layouts[1])
title = slide.shapes.title
title.text = "Slide Title"
prs.save('presentation.pptx')
```

### 슬라이드 원칙
- 1 슬라이드 = 1 메시지
- 텍스트 최소화, 시각적 요소 극대화
- 6×6 규칙: 6줄 이내, 한 줄에 6단어 이내
- 발표자 노트에 상세 설명

## Excel (XLSX)
```python
import openpyxl
wb = openpyxl.Workbook()
ws = wb.active
ws['A1'] = 'Header'
ws['A2'] = 42
wb.save('data.xlsx')
```

### 스프레드시트 작업
- 피벗 테이블 생성
- 차트 삽입
- 수식 작성
- 조건부 서식

## Word (DOCX)
```python
from docx import Document
doc = Document()
doc.add_heading('Title', 0)
doc.add_paragraph('Body text')
doc.save('document.docx')
```

### 문서 작업
- 추적된 변경사항 (Tracked Changes)
- 스타일 적용
- 표 삽입
- 머리글/바닥글

## Task: $ARGUMENTS
