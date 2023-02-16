# Skeletons

로드하는 동안 페이지의 레이아웃을 모방하기 위해 사용자 지정 스켈레톤 위젯을 빌드하기 위한 라이브러리

<br />

## 의존성 추가

[Skeletons](https://pub.dev/packages/skeletons)의 버전을 pub.dev에서 확인하여 pubspec.yaml 파일에 추가한 후 pub get을 해줍니다.

``` yaml
dependencies:
  skeletons: ^[version]
```

<br />

## 사용 이유

페이지에 진입 했을 때 데이터 로드가 끝나지 않아 렌더링 되지 않을 경우 사용자 입장에서 현재 로딩 중인지 아닌지를 확인할 방법이 없습니다.

화면에 전체적으로 로딩 창을 띄우는 방법도 있지만 요즘 트렌드는 페이지의 레이아웃에 맞춰 데이터를 로드하는 부분만 로딩하는 이미지를 주는게 UX가 좋기 때문에 Skeleton 라이브러리를 사용하여 구현할 수 있습니다.

<br />

## 사용 방법

``` dart
// 일반적인 문단 형태의 Skeleton
SkeletonParagraph(
  style: const SkeletonParagraphStyle(
    lines: 4,
  ),
),

// 아바타 이미지 + 문단 형태의 Skeleton
SkeletonListTile(
  hasLeading: true,
  leadingStyle: const SkeletonAvatarStyle(
    width: 110,
    height: 100,
  ),
),
```

> 더 많은 Skeleton 종류와 사용법은 [공식문서](https://pub.dev/packages/skeletons) 참조

<br />

| <img src="https://user-images.githubusercontent.com/68320595/217999746-b05d8cea-a5bd-4dde-9b9b-5857419531d1.gif" /> | <img src="https://user-images.githubusercontent.com/68320595/217999749-54f1e64e-937b-49c9-b0c5-9e04f460b1cf.gif" /> |
| ------------------------------------------------------------ | ------------------------------------------------------------ |

