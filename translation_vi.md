Translating
===========

Nếu bạn muốn sử dụng ngôn ngữ của bạn cho Mastodon, đây là cách làm.

* [Tổng quan](#general)
* [Process](#process)
  * [Obtain the Source Code](#obtain-the-source-code)
  * [Translating](#translating)
  * [Declaring the language](#declaring-the-language)
  * [Sending the translation](#sending-the-translation)
* [Testing the translation](#testing-the-translation)
* [Updating the translation](#updating-the-translation)
* [Appendix](#appendix)
  * [Appendix A. Plural handling](#appendix-a-plural-handling)
  * [Appendix B. Command Tools](#appendix-b-command-tools)

---

## Tổng quan

Mastodon bao gồm 2 thành phần, máy chủ và web client. Các bản dịch cho web client nằm trong [`app/javascript/mastodon/locales`] (https://github.com/tootsuite/mastodon/tree/master/app/javascript/mastodon/locales). Đối với phía máy chủ, các bản dịch nằm trong [`config/locales`] (https://github.com/tootsuite/mastodon/tree/master/config/locales) và được chia thành các tệp khác nhau. Ngoài ra, các mẫu email cho máy chủ nằm trong [`app/views/user_mailer`] (https://github.com/tootsuite/mastodon/tree/master/app/views/user_mailer). Đây là tất cả các tệp bạn cần dịch:
| File gốc (English) | Vị trí | Mô tả |
|---|---|---|
| [`en.json`](https://github.com/tootsuite/mastodon/blob/master/app/javascript/mastodon/locales/en.json) | `app/javascript/mastodon/locales/en.json` | Strings của web client |
| [`en.yml`](https://github.com/tootsuite/mastodon/blob/master/config/locales/en.yml) | `config/locales/en.yml` | Strings dùng chung |
| [`simple_form.en.yml`](https://github.com/tootsuite/mastodon/blob/master/config/locales/simple_form.en.yml) | `config/locales/simple_form.en.yml` | Strings cho phần cài đặt |
| [`devise.en.yml`](https://github.com/tootsuite/mastodon/blob/master/config/locales/devise.en.yml) | `config/locales/devise.en.yml` | String chung của Devise |
| [`doorkeeper.en.yml`](https://github.com/tootsuite/mastodon/blob/master/config/locales/doorkeeper.en.yml) | `config/locales/doorkeeper.en.yml` | String chung của Doorkeeper |
| [`confirmation_instructions.en.html.erb`](https://github.com/tootsuite/mastodon/blob/master/app/views/user_mailer/confirmation_instructions.en.html.erb)<br>[`confirmation_instructions.en.text.erb`](https://github.com/tootsuite/mastodon/blob/master/app/views/user_mailer/confirmation_instructions.en.text.erb) | `app/views/user_mailer/confirmation_instructions.en.html.erb`<br>`app/views/user_mailer/confirmation_instructions.en.text.erb` | Thông báo xác nhận tài khoản cho Devise
| [`password_change.en.html.erb`](https://github.com/tootsuite/mastodon/blob/master/app/views/user_mailer/password_change.en.html.erb)<br>[`password_change.en.text.erb`](https://github.com/tootsuite/mastodon/blob/master/app/views/user_mailer/password_change.en.text.erb) | `app/views/user_mailer/password_change.en.html.erb`<br>`app/views/user_mailer/password_change.en.text.erb` | Thông báo thay đổi mật khẩu cho Devise
| [`reset_password_instructions.en.html.erb`](https://github.com/tootsuite/mastodon/blob/master/app/views/user_mailer/reset_password_instructions.en.html.erb)<br>[`reset_password_instructions.en.text.erb`](https://github.com/tootsuite/mastodon/blob/master/app/views/user_mailer/reset_password_instructions.en.text.erb) | `app/views/user_mailer/reset_password_instructions.en.html.erb`<br>`app/views/user_mailer/reset_password_instructions.en.text.erb`  | Hướng dẫn đặt lại mật khẩu cho  Devise

## Quá trình

### Obtain the Source Code


Nếu bạn sử dụng Github, trước tiên hãy fork Mastodon vào tài khoản của bạn. Sau đó clone nó vào máy của bạn để thực hiện các công việc tiếp theo.
Để biết hướng dẫn chi tiết, bạn có thể đọc [Github cheatsheet](Translating-Github-Cheat-Sheet.md).

### Translating

1. Sao chép các tệp trong thư mục của chúng và thay thế `en` trong tên tệp bằng mã hai chữ cái tiêu chuẩn của ngôn ngữ của bạn
   ([ISO 639-1] (https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)). Hoặc [RFC5646] (https://tools.ietf.org/html/rfc5646)
   thẻ ngôn ngữ cho các ngôn ngữ khu vực.

   Ví dụ: `simple_form.en.yml` trở thành` simple_form.es.yml` trong bản dịch tiếng Tây Ban Nha, và
   `simple_form.zh-HK.yml` trong bản dịch tiếng Trung Phồn thể (HK).

2. Đồng thời thay thế mã ngôn ngữ trong các dòng đầu tiên của tất cả các tệp và dòng cuối cùng của tệp `.js`.

3. Dịch các giá trị bên phải từ tiếng Anh sang ngôn ngữ của bạn. Giữ thụt lề và dấu chấm câu.

Vì Devise và Doorkeeper là những thư viện phổ biến, nên có thể đã có sẵn các tệp dịch cho ngôn ngữ của bạn trên Internet. [Devise's Wiki] (https://github.com/plataformatec/devise/wiki/I18n) và [doorkeeper-i18n] (https://github.com/doorkeeper-gem/doorkeeper-i18n) là những nguồn chính thức tương ứng cho những bản dịch này.

### Declaring the language


Các ngôn ngữ được đề cập trong một số tệp khác. Để kích hoạt bản dịch của bạn, hãy thêm mã ngôn ngữ của bạn vào các danh sách khác nhau có trong các tệp này:
| File | Vị trí | Ghi chú |
|---|---|---|
| [`index.js`](https://github.com/tootsuite/mastodon/blob/master/app/javascript/mastodon/locales/index.js) | `app/javascript/mastodon/locales/index.js` | Thêm 2 dòng |
|[`mastodon.js`](https://github.com/tootsuite/mastodon/blob/master/app/javascript/mastodon/containers/mastodon.js) | `app/javascript/mastodon/containers/mastodon.js` | Thêm 1 dòng + 1 list cần hoàn thành |
| [`settings_helper.rb`](https://github.com/tootsuite/mastodon/blob/master/app/helpers/settings_helper.rb) | `app/helpers/settings_helper.rb` | Thêm 1 dòng + tên ngôn ngữ của bạn |
| [`application.rb`](https://github.com/tootsuite/mastodon/blob/master/config/application.rb) | `config/application.rb` | 1 list cần hoàn thành |

### Sending the translation

Sau đó, bạn có thể push tệp sang git và gửi pull request.


Để biết hướng dẫn chi tiết, bạn có thể đọc [Github cheatsheet](Translating-Github-Cheat-Sheet.md).

## Testing the translation

Khi yêu cầu kéo được chấp nhận, hãy đợi code được triển khai trên phiên bản Mastodon. Đăng nhập bằng tài khoản của bạn ở đó và thay đổi ngôn ngữ trong cài đặt. Duyệt và sử dụng trang web. Xem mọi thứ có hợp lý trong ngữ cảnh hay không và có điều gì không hợp lý hoặc phá vỡ bố cục không. Mời những người dùng Mastodon khác nói ngôn ngữ của bạn dùng thử và đưa ra phản hồi. Thực hiện các thay đổi cho phù hợp và cập nhật bản dịch.

## Updating the translation

Theo dõi các tệp gốc tiếng Anh trong `app/javascript/mastodon/locales` và `config/locales`. Khi chúng được cập nhật, hãy chuyển các thay đổi cho tệp ngôn ngữ của bạn. Đối với các chuỗi mới, hãy thêm các dòng mới vào cùng một vị trí và dịch chúng. Sau khi cập nhật xong, bạn có thể gửi một pull request mới.

## Appendix

### Appendix A. Plural handling

Các ngôn ngữ khác nhau sử dụng các hình thức số nhiều khác nhau được Mastodon xử lý.


Đối với các bản dịch JavaScipt (`.js`), điều này được thực hiện trong [react-intl] (https://github.com/yahoo/react-intl), bằng cách thực hiện:
```
{appleCount, số nhiều, một {là một quả táo} khác {là {appleCount} táo}}.
```

Mặt khác, các tệp `.yml` được xử lý bởi [rails-i18n] (https://github.com/svenfuchs/rails-i18n). Các mục trông giống như thế này là các trường số nhiều:
```YML
eat_apple:
  one: You ate an apple.
  other: You ate %{count} apples.
```

Trong cả hai ví dụ, bạn có thể thấy trường hợp `once` và trường hợp` other` được mô tả cho các chuỗi đa nguyên. Các chuỗi chính xác được chọn theo số lượng bao nhiêu - khi có chính xác một trong số đó, câu chuyển sang trường hợp `một`; nếu không thì nó chuyển sang trường hợp `other`. Đây là cách thức hoạt động của việc plualization đối với tiếng Anh (`en`) và một số ngôn ngữ khác.

Tuy nhiên, có nhiều ngôn ngữ không hoạt động theo cách một chiều. Đánh bóng thành bốn dạng số nhiều, có tên tương ứng là `one`,` few`, `many`, và` other`. Tiếng Ả Rập có sáu. Tiếng Trung, tiếng Nhật và tiếng Hàn chỉ có một dạng được gọi là `other`. Nếu ngôn ngữ của bạn không sử dụng một / các dạng số nhiều khác, hãy nhớ xem phần chính của [Quy tắc Unicode CIDR Số nhiều] (http://www.unicode.org/cldr/charts/28/supplemental/language_plural_rules.html ) đồ thị. Cũng theo nguyên tắc chung, hãy luôn bắt đầu phiên âm với trường hợp `khác` trong các tệp tiếng Anh vì chúng được khái quát tốt hơn so với trường hợp `one`.
### Appendix B. Command Tools

Chúng tôi có các công cụ dòng lệnh để trợ giúp người dịch trong công việc của họ. Chúng thực sự hữu ích.

Để sử dụng các công cụ, bạn cần phải thiết lập đúng môi trường của mình.

#### Setup

Bạn cần có [Ruby] (https://www.ruby-lang.org/en/) và [NodeJS] (https://nodejs.org/en/) thiết lập trong máy của bạn. Nếu bạn muốn giữ cho đường dẫn global của mình sạch sẽ, bạn có thể sử dụng [rvm] (https://rvm.io/) và [nvm] (https://github.com/creationix/nvm).

Bạn cũng cần cài đặt [yarn] (https://yarnpkg.com/) với thiết lập nodejs của mình.

Để cài đặt Ruby với rvm:
```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable
```

Để cài đặt nodejs và yarn với nvm:
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
source ~/.bashrc
npm install stable
npm install -g yarn
```

#### Update node packages and ruby gems

Bạn sẽ cần chạy lệnh này trong thư mục gốc của thư mục mã nguồn của mình:
```
bundle install
yarn install
```

#### Server Translation

Đối với máy chủ dựa trên Ruby, [i18n Tasks] (https://github.com/glebm/i18n-tasks) là
được sử dụng để quản lý các bản dịch.

Câu lệnh:
```
bundle exec i18n-tasks [command] [options]
```

You can use this command to find all usages:
```
bundle exec i18n-tasks --help
```

##### Key Usages

Để xem liệu có bất kỳ khóa ngôn ngữ nào bị thiếu hoặc không sử dụng trong ngôn ngữ của bạn hay không, bạn có thể sử dụng lệnh `sức khỏe`. Ví dụ, nếu bạn muốn kiểm tra sức khỏe của Magyar (** hu **) bản dịch:
```
bundle exec i18n-tasks health hu
```


Nếu bạn thấy rằng có khóa bị thiếu trong tệp của mình, bạn có thể sử dụng `add-missing`:
```
bundle exec i18n-tasks add-missing hu
```

Xin lưu ý rằng `health` sẽ chỉ cần kiểm tra sự tồn tại của khóa ngôn ngữ. Nó không kiểm tra xem chúng có khác với mặc định (tiếng Anh) hay không. Ngoài ra, lệnh `add-missing` chỉ sao chép bản dịch tiếng Anh cho ngôn ngữ của bạn.

Nói tóm lại, chạy `add-missing` có thể giúp bạn vượt qua bài kiểm tra về sức khoẻ`, nhưng bạn vẫn cần kiểm tra yml của bạn và dịch các chuỗi "đã thêm".

#### Web Client Translation

Đối với web client, [tập lệnh NPM] (https://docs.npmjs.com/misc/scripts) được sử dụng.

Có 2 tập lệnh cụ thể mà bạn sẽ phải sử dụng:

**`yarn build:development`**

Xây dựng nội dung webpack để sử dụng cho việc phát triển. Đồng thời, các tạo tập tin reference bản dịch của giao diện người dùng cũng dược sinh ra. Bạn sẽ cần chạy lệnh này mỗi khi bạn clone / fetch mã nguồn.

Bạn sẽ cần chạy lệnh này trước khi sử dụng `yarn manage:translation`.

**`yarn manage:translation`**

Dựa trên [react-intl-translate-manager] (https://www.npmjs.com/package/react-intl-translations-manager).
Đồng bộ hóa và kiểm tra các chuỗi dịch. Nó sẽ:

* Tự động tạo các khóa còn thiếu trong các tệp dịch json; và
* loại bỏ các bản dịch bị lỗi; và
* hiển thị cho bạn danh sách các bản dịch cần thiết; và
* Tạo các tệp dịch không tồn tại (nếu bị buộc phải).

Bạn có thể sử dụng lệnh trợ giúp để nhận hướng dẫn sử dụng:

```
yarn manage:translation -- --help
```

##### Key Usages

Bạn có thể chỉ định ngôn ngữ để đồng bộ hóa và kiểm tra:
```
yarn manage:translation -- [language code]
```

Ví dụ: để đồng bộ hóa bản dịch cho tiếng Pháp (** fr **):
```
yarn manage:translation -- fr
```


Bạn cũng có thể sử dụng điều này để tạo các tệp ngôn ngữ json. Bạn cần phải áp dụng tùy chọn `--force`. Ví dụ: nếu bản dịch javascript (** ar **) tiếng Ả Rập vẫn chưa được tạo:
```
yarn manage:translation -- --force ar
```
sẽ tạo các tệp ngôn ngữ sau:
* app/javascript/mastodon/locales/**ar**.json
* app/javascript/mastodon/locales/whitelist_**ar**.json

#### Check Your Translations


Để xem liệu ngôn ngữ của bạn có hoạt động tốt hay không, bạn có thể kết hợp bằng cách sử dụng các công cụ lệnh, như sau:

```
bundle exec i18n-tasks health zh-HK
yarn manage:translations -- zh-HK
```

Nếu bạn đang làm đúng, bạn sẽ có kết quả như thế này:

```
$ bundle exec i18n-tasks health zh-HK
Forest (zh-HK) has 422 keys in total. On average, values are 13 characters long, keys have 3.2 segments.
✓ Good job! No translations are missing.
✓ Well done! Every translation is in use.

$ yarn manage:translations -- zh-HK
yarn manage:translations v0.23.4
$ node ./config/webpack/translationRunner.js zh-HK
Maintaining zh-HK.json:

 Perfectly maintained, no remarks!

Done in 0.24s.
```