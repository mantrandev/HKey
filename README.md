# HKey Telex

Bộ gõ tiếng Việt cho macOS — fork từ [Mkey](https://github.com/mantrandev/Mkey), bản thân Mkey fork từ [OpenKey](https://github.com/tuyenvm/OpenKey). Stripped down cho cá nhân.

## Tải về

[**HKey-telex-v0.0.5.dmg**](https://github.com/mantrandev/HKey/releases/tag/telex-v0.0.5)

## Screenshot

![HKey menu](docs/screenshot.png)

## Tính năng

- **Kiểu gõ:** Telex (cố định, không có toggle)
- **Bảng mã:** Unicode
- **Phím tắt chuyển ngôn ngữ:** `Ctrl + Space`
- **Menu bar SwiftUI** — hiển thị `H` (tiếng Việt) hoặc `E` (tiếng Anh)

## Yêu cầu

macOS 13.0+ (Ventura), Apple Silicon. Build arm64-only nên không chạy trên Mac Intel.

**Gatekeeper:** Vì app chưa được notarize, cần bỏ chặn thủ công sau khi cài:  
*System Settings → Privacy & Security* → tìm `HKey` → bấm **Open Anyway**.

**Accessibility:** Cấp quyền để app hoạt động:  
*System Settings → Privacy & Security → Accessibility* → bật `HKey`.

**Text Input:** Để HKey hoạt động mượt, chỉ giữ **một** input source là `U.S.` (English) trong *System Settings → Keyboard → Text Input → Input Sources*. Xoá hết các input source tiếng Việt (Telex/VNI) của macOS — HKey tự xử lý phần gõ.

![Text Input config](docs/text-input.png)

## Cài đặt

**Homebrew (khuyến nghị):**

```bash
brew tap mantrandev/tap
brew install --cask mantrandev/tap/hkey-telex
xattr -dr com.apple.quarantine /Applications/HKey.app
```

**Nâng cấp:**

```bash
brew upgrade --cask mantrandev/tap/hkey-telex
xattr -dr com.apple.quarantine /Applications/HKey.app
```

Bước `xattr` là bắt buộc sau **mỗi** lần cài hoặc nâng cấp: Homebrew gắn `com.apple.quarantine` lên app, và vì app chưa notarize nên Gatekeeper chặn không cho mở. Cờ `--no-quarantine` đã bị bỏ từ Homebrew 6, và `HOMEBREW_CASK_OPTS="--no-quarantine"` cũng không còn tác dụng — đã kiểm chứng trên Homebrew 6.0.17.

**Thủ công:**

1. Tải `HKey.dmg` từ [Releases](https://github.com/mantrandev/HKey/releases)
2. Mở DMG, kéo `HKey.app` vào thư mục `Applications`

**Sau khi cài (cả hai cách):**

1. Mở `HKey` — hệ thống sẽ yêu cầu cấp quyền Accessibility
2. Vào *System Settings → Privacy & Security → Accessibility* → bật `HKey`
3. Mở lại `HKey`

## Build

Mở `Sources/macOS/Mkey.xcodeproj`, chọn scheme `Mkey`, build. Target và scheme vẫn giữ tên `Mkey`; app build ra là `HKey.app` qua `PRODUCT_NAME`.

- **Debug:** bundle ID `com.mantrandev.hkey.dev`
- **Release:** bundle ID `com.mantrandev.hkey`

Bundle ID riêng `hkey` để HKey và Mkey cài song song không đè cấu hình, quyền Accessibility và login item của nhau.

Đóng gói DMG:

```bash
./build.sh
```

## Icon

App icon sinh từ vector, không sửa `.icns` bằng tay:

```bash
./design/make-icon.sh
```

Script render `design/Icon.svg` qua Chrome headless ở đủ 10 kích cỡ của iconset (16→1024) rồi `iconutil` đóng thành `Sources/macOS/ModernKey/Resources/Icon.icns`. Sửa icon thì sửa SVG rồi chạy lại script.
