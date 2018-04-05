## Những sai lầm về phát triển dữ liệu mà các nhà phát triển phần mềm thường mắc phải?

_source https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ số thích hợp**

Đây là một điều tưởng chừng như đơn giản nhưng nó lại gặp phải mọi lúc. Khóa ngoại nên có chỉ số trên chúng.Nếu bạn đang sử dụng 1 trường trong mệnh đề 

WHERE bạn nên (có thể) có một chỉ số cho nó. các chỉ mục này thường dựa trên nhiều trường bạn cần để thực hiện truy vấn.

**2. Không thực thi toàn vẹn tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi ngay ở đây nhưng nếu cơ sở dữ liệu của bạn hỗ trợ toàn vẹn tham chiếu--có nghĩa là tất cả khóa ngoại đều đảm bảo trỏ tới thực thể có tồn tại--bạn nên sử dụng nó.

Nó là lỗi khá phổ biến ở cớ sở dữ liệu MySQL. Tôi không tin rằng MyISAM hỗ trợ nó. InnoDB thì có. Bạn sẽ tìm thấy những người sử dụng MyISAM hoặc đang sử dụng InnoDB nhưng sẽ không sử dụng nó (nó ở đây là toàn vẹn tham chiếu) theo bất cứ cách nào.

Đọc thêm ở đây:

- [Các ràng buộc quan trngj như thế nào như NOT NULL và FOREIGN KEY nếu tôi luôn luôn kiểm soát đầu vào của cơ sở dữ liệu trong php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [khóa ngoại thực sự cần thiết trong thiết kế csdl?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Are foreign keys really necessary in a database design?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng khóa chính 1 cách tự nhiên thay vì chỉ để đại diện ( về mặt kỹ thuật)**

Những khóa tự nhiên là những khóa dựa trên ý nghĩa bên ngoài của dữ liệu đó là (bề ngoài) độc nhất.Các ví dụ phổ biến là mã sản phẩm,2 chữa của mã tiểu bang (US), số bảo mật xã hội và vân vân. Các khóa chính thay thế hoặc kỹ thuật chính là những khoá hoàn toàn không có ý nghĩa bên ngoài hệ thống. Chúng được phát minh hoàn toàn để xác định thực thể và thường là các trường gia tăng tự động (SQL Server, MySQL, others) hoặc các chuỗi (đáng chú ý nhất là Oracle).

Theo tôi bạn nên **luôn luôn** sử dụng khóa đại diện. Vấn đề này đưa ra trong những câu hỏi dưới đây:

- [Bạn thích khóa chính của mình như thế nào?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [cách thực hành tốt nhất các khóa chính trong bảng?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Bạn sử dụng định dạng khóa chính nào trong trường hợp này.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Khóa đại diện với khóa tự nhiên / business](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Bạn có nên có trường khóa chính riêng?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là một chủ đề gây nhiều tranh cãi mà bạn sẽ không đạt được thỏa thuận chung. Trong khi bạn có thể tìm thấy một số người, những người nghĩ rằng các khóa tự nhiên là trong một số tình huống OK, bạn sẽ không tìm thấy bất kỳ lời chỉ trích của các khóa đại diện khác hơn là cho là không cần thiết. Đó là một nhược điểm nhỏ nếu bạn hỏi tôi.

Hãy nhớ rằng, [Các quốc gia có thể không tồn tại](http://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ, Yugoslavia).

**4. Viết các truy vấn yêu cầu

DISTINCT làm việc**

Bạn thường thấy điều này trong truy vấn tạo-ORM. Để ý đầu ra log từ Hibernate và bạn sẽ nhìn thấy toàn bộ truy vấn bắt đầu với:

SELECT DISTINCT ...

Đây là một chút của một phím tắt để đảm bảo bạn không trả lại các hàng trùng lặp và do đó nhận được các đối tượng trùng lặp. Đôi khi bạn cũng thấy những người làm việc này. Nếu bạn nhìn thấy nó quá nhiều đó là một lá cờ đỏ thực sự (xuất hiện khi gặp lỗi). Không phải cái đó

DISTINCT là xấu hoặc là không có ứng dụng hợp lệ. Nó có (về cả 2 mặt) Nhưng nó không phải là 1 đại diện hoặc là 1 rào cản cho việc viết truy vấn hợp lệ.

Từ [Tại sao tôi ghét DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Trong trường hợp mọi thứ bắt đầu trở nên rắc rối theo ý kiến ​​của tôi là khi một nhà phát triển đang xây dựng truy vấn đáng kể, Nối các bảng với nhau, và đột nhiên anh ta nhận ra rằng có vẻ như anh ta đang nhận được bản sao (hoặc thậm chí nhiều hơn) hàng và phản ứng ngay lập tức "giải pháp" của anh ta đối với "vấn đề" này là ném vào từ khóa DISTINCT và **POOF** tất cả các rắc rối của ông biến mất..

**5. Khuyến khích tập hợp các kết nối**

Một sai lầm phổ biến khác của các nhà phát triển ứng dụng csdl là không nhận ra các tập hợp giá trị hơn (ví dụ như mệnh đề GROUP BY) có thể được so sánh với joins.

Để cung cấp cho ban một ý tưởng về sự phổ biến rộng rãi này là, Tôi đã viết chủ đề này rất nhiều lần ở đây và bị khá nhiều downvote , Ví dụ:

Từ [Câu lệnh SQL - “join” vs “group by và having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> Truy vấn đầu tiên:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Query time: 0.312 s

> Truy vấn thứ 2:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Query time: 0.016 s

> Nó là đúng. Phiên bản join do tôi đề xuất **nhanh hơn gấp 20 lần phiên bản tổng hợp.**

**6. Không đơn giản hóa các truy vấn phức tạp thông qua các chế độ views**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ quan điểm nhưng đối với những người làm, họ có thể đơn giản hóa các truy vấn nếu được sử dụng một cách thận trọng. Ví dụ, trong một dự án tôi đã sử dụng một [mô hình chung của Party](http://www.tdan.com/view-articles/5014/) đối với CRM. Đây là một kỹ thuật mô hình rất mạnh và linh hoạt nhưng có thể dẫn đến nhiều người tham gia. Trong mô hình này có:

- **Party**: mọi người và tổ chức;
- **Party Role**: những điều mà các bên đã làm, ví dụ Nhân viên và Nhà tuyển dụng;
- **Party Role Relationship**: làm thế nào để những vai trò liên qua đến nhau.

Ví dụ:

- Ted là 1 con người, là 1 phần tử của bữa tiệc;
- Ted có nhiều vai trò, một trong số đó là nhân viên;
- Intel là một tổ chức, là 1 phần tử của bữa tiệc;
- Intel có nhiều vai trò, một trong số đó là nhà tuyển dụng;
- Intel thuê Ted, có nghĩa là có một mối quan hệ tương ứng giữa họ;

Vì vậy, có năm bảng tham gia để liên kết Ted với chủ của mình. Bạn giả định tất cả nhân viên là Người (không phải tổ chức) và cung cấp khung nhìn trợ giúp này:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

và đột nhiên bạn có một cái nhìn đơn giản về dữ liệu mà bạn muốn trên một mô hình có tính linh hoạt cao.

**7. Không chắt lọc đầu vào**

Đây là một lỗi lớn.  Bây giờ tôi thích PHP nhưng nếu bạn không biết bạn đang làm gì thì thật dễ dàng để tạo các trang web dễ bị tấn công. Không có gì tổng kết nó tốt hơn so với [cầu truyện về cái bàn Bobby bé nhỏ](http://xkcd.com/327/).

Dữ liệu được cung cấp bởi người dùng thông qua URLs, form dữ liệu **và cookies** nên luôn luôn được coi là thù địch và khử trùng. Đảm bảo bạn đang nhận được những gì bạn mong đợi.

**8. Không sử dụng câu lệnh đã được chuẩn bị sẵn **

Các câu lệnh đã được chuẩn bị là khi bạn biên dịch truy vấn trừ dữ liệu được sử dụng trong inserts , updates và

mệnh đề WHERE và sau đó cung cấp chúng muộn. Ví dụ như:

SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

hoặc

SELECT * FROM users WHERE username = :username

tùy thuộc vào nền tảng của bạn.

Tôi đã nhìn thấy cơ sở dữ liệu bị sụp xuống bằng cách làm điều này. Về cơ bản, mỗi khi cơ sở dữ liệu hiện đại gặp một truy vấn mới, nó phải biên dịch nó. Nếu nó gặp một truy vấn nó được nhìn thấy trước, bạn đang cho cơ sở dữ liệu cơ hội để cache truy vấn biên dịch và kế hoạch thực hiện. Bằng cách thực hiện các truy vấn rất nhiều bạn đang cho cơ sở dữ liệu cơ hội để con số đó ra và tối ưu hóa cho phù hợp (ví dụ, bằng cách ghim các truy vấn biên dịch trong bộ nhớ).

Sử dụng các câu lệnh chuẩn bị cũng sẽ cung cấp cho bạn số liệu thống kê có ý nghĩa về tần suất các truy vấn nhất định được sử dụng.

Các câu lệnh được chuẩn bị cũng sẽ giúp bạn chống lại các cuộc tấn công SQL injection tốt hơn.

**9. Không chuẩn hóa đủ**

[Chuẩn hóa cơ sở dữ liệu](http://en.wikipedia.org/wiki/Database_normalization) là tiến trình cơ bản của thiết kế cơ sở dữ liệu tối ưu hóa cũng như là cách sắp xếp dữ liệu của bạn vào bảng.

Chỉ trong tuần này Tôi đã chạy qua một vài code nơi mà ai đó đã tách một mảng và đã chèn mảng đó vào trong 1 trường đơn trong cơ sở dữ liệu. Chuẩn hóa có thể biến mảng đấy như thành phần riêng biệt của mảng con (ví dụ quan hệ một - nhiều).

Điều này cũng xuất hiện trong [Phương pháp tốt nhất để lưu trữ list IDs của user](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi đã nhìn thấy ở hệ thống khác danh sách được lưu ở dạng mảng tuần tự.

Nhưng thiếu sót của sự chuẩn hóa cũng xuất hiện trong 1 vài trường hợp.

Xem thêm:

- [Chuẩn hóa: Bao xa là đủ xa?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Tại sao bạn lại cần chuẩn hóa cơ sở dữ liệu ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hóa quá nhiều**

Nó có vẻ như là một sự mâu thuẫn với điều bên trên về chuẩn hóa, như những thứ khác, là một công cụ. **Nó có nghĩa là một kết thúc và không phải là kết thúc của chính nó** . Tôi nghĩ là một vài nhà phát triển quên nó và bắt đầu xử lý một "means" như một "end". Unit testing là một ví dụ điển hình về điều này.

Tôi đã một lần từng làm việc trên một hệ thống mà đã phân cấp rõ ràng cho khách hàng dẫn tới một cái gì đó như:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

Như vậy bạn phải join khoảng 11 bảng lại với nhau trước khi nhận bất cứ dữ liệu gì có nghĩa. Đó là một ví dụ tốt của sự chuẩn hóa đã đi quá xa.

Thêm điểm nữa, hãy cẩn thận và xem xét việc tránh chuẩn hóa có thể có những lợi ích về hiệu suất rất lớn nhưng bạn phải thật sự cẩn thận về điều này.

Xem thêm:

- [Tại sao chuẩn hóa cơ sở dữ liệu quá nhiều là một điều tồi tệ](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Làm thế nào để có thể chuẩn hóa trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [Khi nào sẽ không cần chuẩn hóa cơ sở dữ liệu SQL](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Có thể chuẩn hóa là không bình thường](http://www.codinghorror.com/blog/archives/001152.html)
- [Nguồn gốc của tất cả các cuộc thảo luận chuẩn hóa cơ sở dữ liệu về mã hoá Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. sử dụng các vòng cung độc quyền**

Một vòng cung độc quyền là một lỗi phổ biến nơi một bảng được tạo ra với 2 hay nhiều khóa phụ nơi mà một và chỉ một trong số chúng có thể không rỗng.  **Sai lầm lớn.** Vì một điều nó trở nên khó khăn hơn nhiều để duy trì tính toàn vẹn dữ liệu. Sau tất cả, ngay cả với tính toàn vẹn tham chiếu, không có gì ngăn chặn được hai hoặc nhiều hơn khóa ngoại này thiết lập (phức tạp kiểm tra ràng buộc notwithstanding).

Từ [Một hướng dẫn thực tế về thiết kế dữ liệu quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> Chúng tôi đã chỉ ra rõ ràng về việc chống vòng cung độc quyền ngay lúc nào có thể, đó là lý do tốt rằng họ có thể gặp khó khăn khi viết code và bảo trì.

**12. Không thực hiện phân tích hiệu suất trên tất cả câu truy vấn**

Chủ nghĩa thực dụng thống trị tối cao,đặc biệt trong cả thế giới cơ sở dữ liệu. Nếu bạn đang gắn bó với các quy tắc đến mức nó trở thành 1 thói quen sau đó bạn có thể gặp kha khá sai lầm. Lấy ví dụ về tổng hợp truy vấn như trên. Phiên bản tổng hợp này có thể nhìn "tuyệt" nhưng hiệu suất của nó thật tồi tệ. Nên có một so sánh hiệu suất để kết thúc tranh luận (nhưng nó thì không) nhưng có nhiều luận điểm hơn rằng: tràn ngập những quan điểm không căn cứ ở đầu tiên là không biết gì , thậm chí là nguy hiểm.

**13. Quá phụ thuộc vào UNION ALL và đặc biệt cấu trúc UNION**

Một UNION trong SQL là thuật ngữ chỉ ghép nối các dữ liệu đồng nhất, có nghĩa là chúng có chung kiểu và số cột. Sự khác nhau giữa UNION ALL là một sự ghép nối đơn giản và được ưa thích hơn ở bất cứ nơi nào có thể trong khi một UNION ngầm thực hiện một DISTINCT loại bỏ các dữ liệu trùng lặp.

UNIONs, như DISTINCT,có vai trò của riêng chúng. có các ứng dụng hợp lệ. nhưng nếu bạn nhìn thấy bạn đang làm rất nhiều trong số đó, cụ thể trong truy vấn phụ, có thể bạn đang làm sai.Đó có thể là việc xây dựng truy vấn kém hoặc một mô hình dữ liệu được thiết kế kém buộc bạn phải làm như vậy.

UNIONs, đặc biệt khi sử dụng trong các kết nối hoặc các truy vấn phụ phụ thuộc, có thể làm tê liệt cơ sở dữ liệu. Cố gắng tránh chúng bất cứ khi nào có thể.

**14. Sử dụng điều kiện OR trong các truy vấn**

Điều đó có thể vô hại. Sau tất cả, ANDs là OK. OR cũng OK quá phải không? Sai rồi. Bản chất một điều kiện AND **hạn chế** tập dữ liệu trong khi đó một điều kiện OR **sinh thêm** nó nhưng không theo một cách mà nó đáng để tối ưu hóa. Đặc biệt khi các điều kiện OR khác nhau có thể giao cắt, do đó buộc trình tối ưu hóa có hiệu quả để một hoạt động DISTINCT về kết quả.

Kém:

... WHERE a = 2 OR a = 5 OR a = 11

Tốt hơn:

... WHERE a IN (2, 5, 11)

Bây giờ, trình tối ưu hoá SQL của bạn có thể biến truy vấn đầu tiên thành thứ hai. Nhưng nó có thể không. Chỉ cần không làm điều đó.

**15. Không thiết kế cơ sở dữ liệu của mình để phù hợp với giải pháp hiệu suất cao**

Đây là một điểm khó để định lượng. Nó thường được quan sát bởi hiệu quả của nó. Nếu bạn thấy mình đang viết các truy vấn gnarly cho các tác vụ tương đối đơn giản hoặc các truy vấn để tìm ra thông tin tương đối đơn giản không hiệu quả thì có lẽ bạn có một mô hình dữ liệu nghèo nàn.

Trong một số cách, điểm này tóm tắt tất cả những cái trước đó nhưng nó nhiều hơn một câu chuyện cảnh báo rằng làm những việc như tối ưu hóa truy vấn thường được thực hiện đầu tiên khi nó nên được thực hiện thứ hai. Trước hết bạn phải đảm bảo bạn có một mô hình dữ liệu tốt trước khi cố gắng tối ưu hóa hiệu suất. Như Knuth đã nói:

> Tối ưu hóa sớm là gốc rễ của tất cả các điều tệ hại

**16. Sử dụng Database Transactions sai cách**

Tất cả các dữ liệu thay đổi cho một quá trình cụ thể phải là rất nhỏ. Ví dụ. Nếu hoạt động thành công, nó sẽ làm đầy đủ. Nếu không thành công, dữ liệu sẽ không thay đổi. - Không nên có những thay đổi "hoàn thành một nửa".

ý tưởng nhất, cách đơn giản nhất để đạt được điều này là toàn bộ thiết kế hệ thống nên cố gắng hỗ trợ tất cả các thay đổi dữ liệu thông qua các câu lệnh INSERT / UPDATE / DELETE. Trong trường hợp này, không có xử lý giao dịch đặc biệt là cần thiết, vì động cơ cơ sở dữ liệu của bạn nên tự động làm như vậy.

Tuy nhiên, nếu bất kỳ quy trình nào yêu cầu nhiều lệnh được thực hiện như là một đơn vị để giữ dữ liệu ở trạng thái nhất quán, thì cần điều khiển giao dịch thích hợp.

- Begin a Transaction trước đầu tiên.
- Commit the Transaction  sau cuối cùng.
- Trên bất kỳ lỗi nào, hãy Hoàn tác Giao dịch. Và rất NB! Đừng quên bỏ qua / hủy bỏ tất cả các câu lệnh theo sau lỗi.

Cũng nên chú ý cẩn thận đến các subtelties của lớp kết nối cơ sở dữ liệu của bạn, và công cụ cơ sở dữ liệu tương tác trong vấn đề này. 

**17. Chưa nắm rõ mô hình 'set-based'**

Ngôn ngữ SQL theo mô hình cụ thể phù hợp với các vấn đề cụ thể. Mặc dù nhiều phần mở rộng cụ thể của nhà cung cấp, Ngôn ngữ đấu tranh để giải quyết vấn đề tầm thường trong ngôn ngữ như Java, C#, Delphi etc.

Sự thiếu hiểu biết này thể hiện theo một vài cách.

- Áp dụng không chính đáng quá nhiều quy tắc và bắt buộc trên vùng dữ liệu.
- Sử dụng con trỏ không phù hợp hoặc 1 cách quá đáng. Đặc biệt khi mà chỉ một câu truy vấn đơn là đủ.
- Giả định không chính xác do đó gây nên fire một lần mỗi hàng bị ảnh hưởng trong cập nhật multi-row.

Xác định phân chia công việc việc rõ ràng và cố gắng sử dụng những công cụ chính xác để giải quyết vấn đề.
