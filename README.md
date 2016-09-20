Xin chào các bạn. hôm nay mình sẽ hướng dẫn các bạn cách [ xây dựng website wordpress bằng Microsoft Azure](http://hieutran.xyz/tao-trang-web-wordpress-bang-microft-azure.html) thông qua bộ cài **WordPress** lên **Web App Azure** cho các bạn sử dụng Dreamsparkđể kích hoạt.

Trước tiên mình xin giải thích lý do vì sao có bài viết này : là vì ở **Marketplace Azure Student for Dreamspark Viet Nam** (dành riêng cho sinh viên Việt Nam) không hỗ trợ tạo **MySQL Database** hay **MSSQL Database** do đó bạn sẽ không thể tạo được các **website **sử dụng **Database**. Chính vì lý do đó mà hôm nay mình sẽ hướng dẫn các bạn **cách cài WordPress lên Web App Azure** sử dụng** Database** là **SQLite**.

**SQLite**

Theo trang sqlite.com, SQLite là một thư viện thực thi các chức năng của một database engine với đặc điểm giống như một phần mềm portable, không cần cài đặt, không cần cấu hình, không cần server, những điểm này rất khác so với việc sử dụng SQL Server hoặc Oracle, nhưng nó vẫn có transaction để đảm bảo tính toàn vẹn và an toàn trong quá trình thao tác dữ liệu. Có thể so sánh nó có một vài điểm giống với Access, nhưng nhìn chung vẫn có nhiều sự khác biệt.

**SQL phù hợp với tình huống nào? Có thay thế được cho SQL Server, Oracle, Postgre… được không?**
**Trả lời luôn là không.**
**SQLite phù hợp trong các tình huống sau:**

**1.Ứng dụng sử dụng dạng flat file để lưu trữ dữ liệu:** như tra cứu  từ điển, các ứng dụng nhỏ, lưu trữ cấu hình  ứng dụng,lưu trữ dữ liệu… Nó mạnh mẽ hơn nhiều kỹ thuật lưu trữ file thông thường.
**2.Sử dụng trong các thiết bị nhúng:** smart phone, PDA và các thiết bị di động rất phù hợp với SQLite.
Các website có khoảng 100.000 lượt xem/ngày, mặc dù về mặt lý thuyết thì SQLite có thể đáp ứng gấp 10 lần con số này.
**3.Là thay thế hoàn hảo cho database dạng file:** nhiều ứng dụng sử dụng fopen(), fread(), fwite() để lưu trữ dữ liệu, SQLite sẽ thay thế hoàn toàn kỹ thuật đó bằng kỹ thuật hiện đại hơn, dễ dùng và tin cậy hơn.
**4.Sử dụng làm database tạm để lưu trữ dữ liệu lấy về từ các database trên SQL server, Oracle…**
Làm database demo cho các ứng dụng lớn, dùng để làm mô hình khái niệm cho ứng dụng.
**5.Dùng cho giảng dạy:** để giảng dạy cho những người mới làm quen với ngôn ngữ truy vấn SQL.

Ngoài các trường hợp trên, tốt hơn hết là bạn sử dụng SQL Server, Oracle, PostgreSQL để xây dựng các ựng dụng cho doanh nghiệp.

Vậy là các bạn đã nắm sơ qua về ứng dụng của SQLite,tiếp theo chúng ta sẽ bắt đầu xây dựng website WordPress bằng SQLite.

**Bước 1:**Trước tiên các bạn Upload toàn bộ file cài của mình lên Web App. Ở bước này thì tùy bạn chọn cách Upload của mình như thế nào là phù hợp và thuận tiện nhất (do Web App Azure mặc định đã bật sẵn PHP version 5.4 nên về vấn đề cấu hình máy chủ như thế nào thì không cần quan tâm lắm)

Ở đây mình sẽ sử dụng cách thức Upload File bằng FTP cho nó đơn giản và dễ sử dụng.

Các bạn tải bộ cài WordPress về giải nén ra sẽ được thư mục WordPress, tìm file có tên là **wp-config-sample.php** và đổi tên nó thành **wp-config.php**.

Sau đó mở file đó lên bằng các chương trình Editor chẳng hạn như **Visual Studio Code (Free)** hay **Notepad++ **để chỉnh sửa file này. Trong ví dụ này mình sẽ sử dụng Notepad++ vì giao diện dễ nhìn và trực quan hơn.

Tiếp theo các bạn truy cập đường [đường link này](https://api.wordpress.org/secret-key/1.1/salt/) để lấy mã Salt dùng trong việc bảo mật kết nối CSDL,… nói chung là giúp Blog của bạn tránh bị hacker xâm nhập. Sau khi truy cập bạn sẽ nhận được một đống ký tự, copy hết đống ký tự đó và mở file wp-config.php lên tìm đến đoạn  put your unique phrase here thì sẽ thấy như thế này

> define('AUTH_KEY',         'put your unique phrase here');
> define('SECURE_AUTH_KEY',  'put your unique phrase here');
> define('LOGGED_IN_KEY',    'put your unique phrase here');
> define('NONCE_KEY',        'put your unique phrase here');
> define('AUTH_SALT',        'put your unique phrase here');
> define('SECURE_AUTH_SALT', 'put your unique phrase here');
> define('LOGGED_IN_SALT',   'put your unique phrase here');
> define('NONCE_SALT',       'put your unique phrase here');
Và thay bằng:
> define('AUTH_KEY',         '4JQB$mY0Dkv|qClnq !m7+>3&3s8+:@c.)Cn-+~Fs*n-d&yf)eMR b!WQ# 3pu,t');
> define('SECURE_AUTH_KEY',  'gI%n5BrCKe:tcDDH+d)#PSOE+{{#EB:L`=(`L#~amIks[^$a&MtJyIY=oS_f#a');
> define('LOGGED_IN_KEY',    '1.Hf:K>|i?V:<FP:BD`M`|ZUJ#L=Gy@<MQergv2-sm>Ee.-z_,Dz+g./OLW_] +Q');
> define('NONCE_KEY',        'jw^w2`0@)^]>395K.;f96*:IrTtK61n8feymor=I:>f#MKZ>,w%U)e$+o*+_!wwa');
> define('AUTH_SALT',        'F?c0<Cze|pb6q-/@mL AzZVI;-I6_^X!!%K3HIM.o9CaRFu3D>6%}#cHcxiu|z~ ');
> define('SECURE_AUTH_SALT', '5d@EZ*D?yJ*:@6=2c ,EA;pT8@r{>!5hi?,8E0/QN^NX[K!ert^mVU9YLE<!+^j^');
> define('LOGGED_IN_SALT',   'bAZYV8F4ZFZuRdD5VJW}kiD70(Or568QyA+j9:*|J}*%+0+x7*A)gZ+,[^bPnQq6');
> define('NONCE_SALT',       'WU--1aiQ,t$l BL_-2u_JH8:?S.KQ=YH?Q^t((hmaw]O0iUtgi1_##]8-RU3g+kg');

Các bạn Save file này lại là xong bước 1.

**Bước 2 :** Các bạn giải nén **SQLite Integration** đã tải ở trên về và copy vào thư mục** wp-content/plugins**.

Tiếp theo các bạn vào thư mục wp-content/plugins/sqlite-integration sẽ thấy file tên là db.php , các bạn Move file đó ra thư mục wp-content. Sau khi hoàn thành thì cấu trúc file và thư mục sẽ như sau : ( ở bước này nếu các bạn đã tải bản do mình Config sẵn thì chỉ cần copy toàn bộ file giải nén được vào thư mục wp-content là xong).Bạn nào chưa có thì download SQLite Integration

**Bước 3 :** Các bạn Upload toàn bộ file trong thư mục wordpress vào thư mục wwwroot qua FTP là hoàn tất gần hết chặng đường.

Sau khi các bạn upload thành công các bạn sẽ được như hình bên dưới.

lưu ý: Bạn không cần cài đặt SQLite Integration vì mình đã cấu hình toàn bộ trong bộ source mà bạn đã download.


 
