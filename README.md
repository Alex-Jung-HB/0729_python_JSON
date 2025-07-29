# PeopleNet 코드에서 JSON 사용 이유

## peoplenet_analysis.json 파일 내용

---

## 1. JSON이 뭔가요?

### JSON = 데이터를 정리해서 저장하는 방법

일상 예시로 설명하면:
* **학생부** 같은 거예요
* 학생 이름, 점수, 과목을 **규칙에 맞게 정리**해서 적어놓은 것

---

## 2. JSON 구조 (기본 문법)

### 기본 규칙
```json
{
  "키": "값"
}
```

### 실제 예시 - 학생 정보
```json
{
  "이름": "김철수",
  "나이": 20,
  "점수": 85,
  "과목들": ["수학", "영어", "과학"],
  "졸업여부": false
}
```

---

## 3. 우리 PeopleNet에서 JSON 사용법

### 동영상 1프레임의 정보
```json
{
  "frame": 0,           // ← 몇 번째 프레임인지
  "timestamp": 0.0,     // ← 몇 초 지점인지  
  "people_count": 2,    // ← 이 프레임에 사람이 몇 명인지
  "detections": [       // ← 검출된 사람들의 상세 정보
    {
      "bbox": [100, 50, 200, 300],  // ← 사람이 있는 위치 (x1,y1,x2,y2)
      "confidence": 0.85,           // ← 얼마나 확실한지 (85%)
      "class": "person"             // ← 뭘 검출했는지
    }
  ]
}
```

### 전체 동영상 정보 (여러 프레임)
```json
[
  {
    "frame": 0,
    "timestamp": 0.0,
    "people_count": 2,
    "detections": [...]
  },
  {
    "frame": 1,
    "timestamp": 0.033,
    "people_count": 1,
    "detections": [...]
  },
  {
    "frame": 2,
    "timestamp": 0.066,
    "people_count": 3,
    "detections": [...]
  }
]
```

---

## 4. 왜 JSON을 쓰나요?

### Before (JSON 없이)
```
프레임0 사람2명 위치100,50,200,300 확률85% 위치300,80,400,320 확률92%
프레임1 사람1명 위치150,60,250,310 확률88%
```
**👎 읽기 어렵고 처리하기 복잡**

### After (JSON 사용)
```json
[
  {
    "frame": 0,
    "people_count": 2,
    "detections": [
      {"bbox": [100,50,200,300], "confidence": 0.85},
      {"bbox": [300,80,400,320], "confidence": 0.92}
    ]
  }
]
```
**👍 깔끔하고 처리하기 쉬움**

---

## 5. 실제로 어떻게 활용하나요?

### 데이터 분석

```python
# JSON 파일 열기
import json
with open('peoplenet_analysis.json', 'r') as f:
    data = json.load(f)

# 질문: 10초 지점에 사람이 몇 명 있었나?
for frame_info in data:
    if frame_info['timestamp'] >= 10.0:
        print(f"10초 지점 사람 수: {frame_info['people_count']}명")
        break

# 질문: 가장 많이 사람이 검출된 순간은?
max_people = 0
max_time = 0
for frame_info in data:
    if frame_info['people_count'] > max_people:
        max_people = frame_info['people_count']
        max_time = frame_info['timestamp']
        
print(f"최대 {max_people}명이 {max_time}초에 검출됨")
```

---

## 6. 앙상블에서 JSON 활용

나중에 세 모델을 합칠 때:

```json
{
  "frame": 0,
  "timestamp": 0.0,
  "peoplenet_result": {
    "people_count": 2,
    "detections": [...]
  },
  "trafficnet_result": {
    "cars_count": 1,
    "detections": [...]
  },
  "yolo_result": {
    "objects_count": 5,
    "detections": [...]
  },
  "final_result": {
    "combined_count": 8,
    "best_detections": [...]
  }
}
```

---

## 💡 핵심 요약

**JSON은 데이터를 체계적으로 정리해서 나중에 쉽게 찾고 분석할 수 있게 해주는 도구입니다!** 📋✨

마치 **정리가 잘 된 노트**와 같다고 생각하시면 됩니다!

---

## 📊 JSON 사용의 장점

| **장점** | **설명** | **예시** |
|---------|---------|---------|
| **구조화** | 데이터가 체계적으로 정리됨 | 프레임별 정보 분류 |
| **가독성** | 사람이 읽기 쉬움 | 키-값 구조로 명확함 |
| **프로그래밍** | 코드로 쉽게 처리 가능 | Python, JavaScript 지원 |
| **확장성** | 나중에 항목 추가 용이 | 새로운 모델 결과 추가 |
| **표준화** | 널리 사용되는 형식 | 다른 프로그램과 호환 |

---

## 🚀 실제 활용 시나리오

### 시나리오 1: 사람 수 변화 분석
```python
# 시간대별 사람 수 변화 그래프 그리기
import matplotlib.pyplot as plt

times = [frame['timestamp'] for frame in data]
counts = [frame['people_count'] for frame in data]

plt.plot(times, counts)
plt.title('시간대별 사람 수 변화')
plt.xlabel('시간(초)')
plt.ylabel('사람 수')
plt.show()
```

### 시나리오 2: 특정 조건 검색
```python
# 사람이 5명 이상 검출된 시점 찾기
crowded_moments = []
for frame in data:
    if frame['people_count'] >= 5:
        crowded_moments.append({
            'time': frame['timestamp'],
            'count': frame['people_count']
        })

print("혼잡한 순간들:")
for moment in crowded_moments:
    print(f"  {moment['time']:.2f}초: {moment['count']}명")
```

### 시나리오 3: 신뢰도 기반 필터링
```python
# 높은 신뢰도의 검출만 추출
high_confidence_detections = []
for frame in data:
    for detection in frame['detections']:
        if detection['confidence'] > 0.9:  # 90% 이상 신뢰도
            high_confidence_detections.append({
                'frame': frame['frame'],
                'bbox': detection['bbox'],
                'confidence': detection['confidence']
            })

print(f"고신뢰도 검출 개수: {len(high_confidence_detections)}")
```

이렇게 JSON을 사용하면 PeopleNet의 검출 결과를 효율적으로 저장하고 다양한 방식으로 분석할 수 있습니다! 🎯
