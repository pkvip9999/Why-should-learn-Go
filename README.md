
[Source](https://medium.com/exploring-code/why-should-you-learn-go-f607681fad65 "Permalink to Why should you learn Go? – Exploring Code – Medium")

# Tại sao bạn nên học Go?
Trong vài năm qua, có một sự tăng trưởng của ngôn ngữ lập trình mới: Go hoặc GoLang. Không có gì làm cho developer trở nên điên rồ hơn một ngôn ngữ lập trình mới, đúng không? Vì vậy, tôi bắt đầu học Go trước 4 đến 5 tháng và ở đây tôi sẽ cho bạn biết lý do tại sao bạn cũng nên học ngôn ngữ mới này.

Tôi sẽ không dạy bạn, cách bạn có thể viết “Hello World !!” trong bài viết này. Có rất nhiều bài báo online cho điều đó. Tôi sẽ giải thích giai đoạn hiện tại của phần mềm máy tính và tại sao chúng ta cần ngôn ngữ mới như Go? Bởi vì nếu không có vấn đề gì thì chúng ta không cần giải pháp, đúng không?
### **Giới hạn về phần cứng:**
Bộ xử lý Pentium 4 đầu tiên với tốc độ xung nhịp 3.0GHz đã được Intel giới thiệu vào năm 2004. Hiện giờ, Mackbook Pro 2016 của tôi có tốc độ xung nhịp 2,9GHz. Vì vậy, gần một thập kỷ, không có quá nhiều lợi ích trong việc tăng sức mạnh xử lý . Bạn có thể thấy so sánh việc tăng sức mạnh xử lý với thời gian biểu đồ bên dưới

Từ biểu đồ trên, bạn có thể thấy rằng hiệu suất của một luồng đơn và tần số của bộ xử lý vẫn ổn định trong gần một thập kỷ. Nếu bạn đang nghĩ rằng việc thêm nhiều transistor là giải pháp, thì bạn đã sai. Điều này là do ở quy mô nhỏ hơn, một số tính chất lượng tử bắt đầu nổi lên (như đường hầm) và vì nó thực sự tốn nhiều tiền hơn để đặt nhiều transistor hơn (tại sao?) Và số transistor bạn có thể thêm vào mỗi đô la bắt đầu giảm.

Vì vậy, để giải quyết vấn đề trên,
  - Các nhà sản xuất bắt đầu bổ sung thêm nhiều nhân hơn cho bộ vi xử lý. Ngày nay chúng tôi có sẵn các CPU 4 nhân và 8 nhân 
  - Chúng tôi cũng giới thiệu về siêu luồng.
  - Đã thêm bộ nhớ cache vào bộ xử lý để tăng hiệu suất
  
  Nhưng các giải pháp trên cũng có những hạn chế riêng. Chúng tôi không thể thêm nhiều bộ nhớ đệm và nhiều hơn nữa để bộ xử lý tăng hiệu suất như bộ nhớ đệm có giới hạn vật lý: bộ nhớ cache càng lớn, nó càng chậm. Thêm nhiều nhân hơn cho bộ vi xử lý cũng có chi phí của nó. Ngoài ra, điều đó không thể mở rộng đến vô thời hạn. Các bộ vi xử lý đa nhân này có thể chạy đồng thời nhiều luồng và mang lại sự đồng thời về hình ảnh. Chúng ta sẽ thảo luận sau.
  
Vì vậy, nếu chúng ta không thể dựa vào những cải tiến phần cứng, cách duy nhất để đi là phần mềm hiệu quả hơn để tăng hiệu suất. Nhưng thật đáng buồn, ngôn ngữ lập trình hiện đại không hiệu quả lắm.

  > “Bộ vi xử lý hiện đại giống như những chiếc xe hơi vui nhộn chạy bằng nitro, chúng vượt trội trong một phần tư dặm. Thật không may là các ngôn ngữ lập trình hiện đại giống như Monte Carlo, chúng có đầy đủ các vòng xoắn và quay. ”- David Ungar
### **Go có goroutines !!**

Như chúng ta đã thảo luận ở trên, các nhà sản xuất phần cứng đang thêm nhiều nhân hơn vào các bộ vi xử lý để tăng hiệu năng. Tất cả các trung tâm dữ liệu đang chạy trên các bộ vi xử lý đó và chúng ta sẽ mong đợi sự gia tăng về số nhân trong những năm sắp tới. Thêm vào đó, các ứng dụng ngày nay sử dụng nhiều micro-services để duy trì kết nối cơ sở dữ liệu, message queues và lưu trữ cache. Vì vậy, phần mềm chúng tôi phát triển và các ngôn ngữ lập trình nên hỗ trợ đồng thời một cách dễ dàng và chúng phải có khả năng mở rộng với số lượng nhân tăng lên.

Tuy nhiên, hầu hết các ngôn ngữ lập trình hiện đại (như Java, Python, vv) là từ môi trường luồng đơn 90 '. Hầu hết các ngôn ngữ lập trình đều hỗ trợ đa luồng. Nhưng vấn đề thực sự đi kèm với việc thực hiện đồng thời,threading-locking,, race conditions và deadlocks. **Những điều này làm cho việc tạo một ứng dụng đa luồng trên các ngôn ngữ đó trở nên khó khăn.**

Ví dụ, tạo một luồng mới trong Java thực sự không hiệu quả về bộ nhớ. Vì mỗi luồng tiêu tốn khoảng 1MB kích thước bộ nhớ heap và cuối cùng nếu bạn bắt đầu chạy hàng nghìn luồng, chúng sẽ gây áp lực rất lớn trên heap và sẽ gây ra shut down do tràn bộ nhớ. Ngoài ra, nếu bạn muốn giao tiếp giữa hai hoặc nhiều luồng, thì sẽ rất khó khăn.

Mặt khác, Go được phát hành vào năm 2009 khi bộ vi xử lý đa nhân đã có sẵn. Đó là lý do tại sao Go được xây dựng với việc lưu giữ đồng thời trong tâm trí. Go có goroutines thay vì threads. Chúng tiêu tốn gần 2KB bộ nhớ từ heap. Vì vậy, bạn có thể chạy hàng triệu goroutines bất cứ lúc nào.

![][1]

Goroutines hoạt động như thế nào? Reffrance: 

**Các lợi ích khác là :**

* Goroutines có segmented stacks có thể mở rộng. Điều đó có nghĩa là chúng ta sẽ chỉ sử dụng thêm bộ nhớ hơn khi cần thiết.
* Goroutines có thời gian khởi động nhanh threads.
* Goroutines đi kèm với built-in primitives để giao tiếp an toàn giữa chúng (kênh).
* Goroutines cho phép bạn tránh mutex locking khi chia sẻ cấu trúc dữ liệu.
* Ngoài ra, các goroutines và OS threads không có ánh xạ 1: 1. Một goroutine đơn có thể chạy trên nhiều luồng. Goroutine được ghép thành một số lượng nhỏ của OS threads..

> Bạn có thể xem cuộc nói chuyện tuyệt vời của Rob Pike [đồng thời chứ không phải song song][2] để hiểu sâu hơn về điều này.

Tất cả các điểm trên, làm cho Go rất mạnh mẽ để xử lý đồng thời như Java, C và C ++ trong khi vẫn giữ code thực thi  đồng thời và đẹp như Erlang.

![][3]

Go làm tốt cả hai thế giới. Dễ dàng viết đồng thời và hiệu quả để quản lý đồng thời

### **Chạy trực tiếp trên nển tảng phần cứng.**

Một lợi ích đáng kể nhất của việc sử dụng C, C ++ so với các ngôn ngữ bậc cao hiện đại khác như Java / Python là hiệu năng của chúng. Bởi vì C / C ++ được biên dịch và không phải thông dịch.

Bộ xử lý hiểu các mã nhị phân. Nói chung, khi bạn xây dựng một ứng dụng bằng cách sử dụng Java hoặc các ngôn ngữ dựa trên JVM khác khi biên dịch dự án của bạn, nó biên dịch code có thể đọc được thành  byte-code mà có thể được JVM hoặc các máy ảo khác chạy trên nền tảng OS. Trong khi thực thi, VM sẽ phiên dịch bytecode đó và chuyển chúng thành mã nhị phân mà bộ xử lý có thể hiểu được

Trong khi ở phía bên kia, C / C ++ không thực hiện trên máy ảo và loại bỏ một bước từ chu kỳ thực hiện và tăng hiệu suất. Nó trực tiếp biên dịch code có thể đọc được của con người thành mã nhị phân.

Nhưng, giải phóng và phân bổ biến trong những ngôn ngữ đó là một khó khăn lớn. Trong khi hầu hết các ngôn ngữ lập trình xử lý phân bổ đối tượng và loại bỏ bằng cách sử dụng Garbage Collector hoặc thuật toán Reference Counting.

Go mang đến những điều tốt nhất của cả hai thế giới. Giống như các ngôn ngữ cấp thấp hơn như C / C ++, Go là ngôn ngữ được biên dịch. Điều đó có nghĩa là hiệu suất gần như gần hơn với các ngôn ngữ cấp thấp hơn. Nó cũng sử dụng garbage collection để phân bổ và loại bỏ đối tượng. Vì vậy, không có các câu lệnh malloc() và free()!!! Thật tuyệt!!! 
### **Code được viết bằng Go rất dễ bảo trì.**
Hãy để tôi nói với bạn một điều. Go không có cú pháp lập trình điên rồ như các ngôn ngữ khác. Nó có cú pháp rất gọn gàng và rõ ràng.

Các nhà thiết kế của Go tại google luôn nghĩ về điều này trong tâm trí khi họ tạo ra ngôn ngữ này. Vì google có code base rất lớn và hàng nghìn developer đang làm việc trên cùng một code base đó nên code phải đơn giản và dễ hiểu cho các developer khác và một đoạn code nên có ảnh hưởng tối thiểu trên một đoạn code khác. Điều đó sẽ làm cho code dễ bảo trì và dễ sửa đổi.

Go chủ động bỏ qua nhiều tính năng của ngôn ngữ OOP hiện đại.

  * Không có các class. Mọi thứ được chia thành các package. Go chỉ có structs thay vì class.
  * Không hỗ trợ kế thừa. Điều đó sẽ làm cho code dễ sửa đổi. Trong các ngôn ngữ khác như Java/Python, nếu lớp ABC kế thừa lớp XYZ và bạn thực hiện một số thay đổi trong lớp XYZ, thì có thể tạo ra một số ảnh hưởng trong các lớp khác kế thừa XYZ. Bằng cách loại bỏ kế thừa, Go làm cho  dễ hiểu code hơn (vì không có super class  để xem xét trong khi nhìn vào một đoạn code).
  * Không constructors.
  * Không annotations.
  * Không generics.
  * Không exceptions.
  
  Những thay đổi trên làm cho Go rất khác với các ngôn ngữ khác và nó làm cho việc lập trình bằng Go khác với các ngôn ngữ khác. Bạn có thể không thích một số điểm ở bên trên. Tuy nhiên, nó không giống như bạn không thể code ứng dụng của bạn mà không có các tính năng trên. Tất cả những gì bạn phải làm là viết 2-3 dòng nữa. Nhưng về mặt tích cực, nó sẽ làm cho code của bạn sạch hơn và rõ ràng hơn.
  
Biểu đồ trên hiển thị rằng Go gần như hiệu quả như C/C++, trong khi vẫn giữ cú pháp code đơn giản như Ruby, Python và các ngôn ngữ khác. Đó là một trường hợp có lợi cho cả con người và bộ vi xử lý !!!

Không giống như các ngôn ngữ mới khác như Swift, cú pháp của Go rất ổn định. Nó vẫn như cũ kể từ khi phát hành công khai ban đầu 1.0, từ  năm 2012. Điều đó làm cho nó có tính tương thích ngược.

### **Go được hỗ trở bởi Google.**
Tôi biết đây không thực sự phải là một lợi thế kỹ thuật. Tuy nhiên, Go được thiết kế và hỗ trợ bởi Google. Google có một trong những cơ sở hạ tầng cloud  lớn nhất trên thế giới và nó có khả năng mở rộng. Go được thiết kế bởi Google để giải quyết các vấn đề của họ về hỗ trợ khả năng mở rộng và hiệu quả. Đó là những vấn đề tương tự bạn sẽ phải đối mặt trong khi tạo ra các máy chủ của riêng bạn.

Thêm vào đó Go cũng được sử dụng bởi một số công ty lớn như Adobe, BBC, IBM, Intel và cả Medium.(Source: https://github.com/golang/go/wiki/GoUsers)

### **Phần kết luận:**
Mặc dù Go rất khác với các ngôn ngữ hướng đối tượng khác, nó vẫn là cùng một loại. Go cung cấp cho bạn hiệu suất cao như C/C++, xử lý đồng thời siêu hiệu quả như Java và thú vị với code như Python/Perl.

Nếu bạn không có kế hoạch học Go, tôi vẫn sẽ nói giới hạn phần cứng đặt áp lực cho chúng tôi, các developer phần mềm để viết code siêu hiệu quả. Developer cần hiểu phần cứng và làm cho chương trình của họ tối ưu hóa phù hợp. Phần mềm được tối ưu hóa có thể chạy trên phần cứng rẻ hơn và chậm hơn (như thiết bị IOT) và tác động tổng thể tốt hơn đến trải nghiệm người dùng cuối.

[1]: https://cdn-images-1.medium.com/max/1600/1*NFojvbkdRkxz0ZDbu4ysNA.jpeg
[2]: https://blog.golang.org/concurrency-is-not-parallelism
[3]: https://cdn-images-1.medium.com/max/1600/1*xbsHBQJReC5l_VO4XgNSIQ.png

  
