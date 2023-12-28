# webview_flutter IOS Alert 이슈 해결법

webview_flutter 라이브러리로 웹뷰를 열었을 때 IOS에서 Alert, Confirm창이 열리지 않는 이슈가 있습니다.

이에 따라 webview_flutter 라이브러리의 IOS 네이티브 코드를 수정해줘야 정상적으로 알림창이 열립니다.

<br />

## 해결 방법

### 1. Xcode 접속

### 2. Pods > Developments Pods > webview_flutter_wkwebview > .. > Classes > FWFUIDelegateHostApi.m 파일 접속

###      2-1. 위의 경로 중 `..`이 있는데 생각보다 `..` 디렉토리가 깊게 구성 되어 있으니 `Classes` 디렉토리가 나오기 전

###              까지 계속 하위 디렉토리를 열면 됩니다.

### 3. FWFUIDelegateHostApi.m 파일에 접속하여 아래의 코드를 `@implementation FWFUIDelegate`와 `@end` 사이에 추가

``` swift
- (void)webView:(WKWebView *)webView runJavaScriptAlertPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(void))completionHandler{
UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"" message:message?:@"" preferredStyle:UIAlertControllerStyleAlert];
  [alertController addAction:([UIAlertAction actionWithTitle:@"확인" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
  completionHandler();
  }])];

    UIViewController *_viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
  [_viewController presentViewController:alertController animated:YES completion:nil];
}

- (void)webView:(WKWebView *)webView runJavaScriptConfirmPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(BOOL))completionHandler{
  UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"" message:message?:@"" preferredStyle:UIAlertControllerStyleAlert];
  [alertController addAction:([UIAlertAction actionWithTitle:@"취소" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
    completionHandler(NO);
  }])];
  [alertController addAction:([UIAlertAction actionWithTitle:@"확인" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
    completionHandler(YES);
  }])];

    UIViewController *_viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
  [_viewController presentViewController:alertController animated:YES completion:nil];
}

- (void)webView:(WKWebView *)webView runJavaScriptTextInputPanelWithPrompt:(NSString *)prompt defaultText:(NSString *)defaultText initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(NSString * _Nullable))completionHandler{
  UIAlertController *alertController = [UIAlertController alertControllerWithTitle:prompt message:@"" preferredStyle:UIAlertControllerStyleAlert];
  [alertController addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
    textField.text = defaultText;
  }];
  [alertController addAction:([UIAlertAction actionWithTitle:@"" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
    completionHandler(alertController.textFields[0].text?:@"");
  }])];
  UIViewController *_viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
  [_viewController presentViewController:alertController animated:YES completion:nil];
}
```

<br />

## 유의 사항

1. 해당 방법은 webview_flutter 라이브러리의 문제로 인한 임시방편 해결입니다.

2. 해당 이슈를 수정한 라이브러리 버전이 나오면 그 버전으로 라이브러리를 업데이트 하는게 가장 좋습니다.

3. 해당 코드는 Pods 하위에 있기 때문에 `ios/.gitignore` 파일에 포함되어 있어 Git에 올리지 못하여 형상관리를 할 수 없습니다.

4. 배포 및 라이브러리 업데이트를 할 때 위의 코드가 반영이 되었는지 확인해야 합니다.
