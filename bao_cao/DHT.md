# Giới thiệu về mạng peer-to-peer, Distributed Hash Table(DHT), Chord
- Bài viết mở đầu là giới thiệu tổng quan về mạng Peer-To-Peer
- Tiếp theo là giới thiệu về mạng Peer-To-Peer có cấu trúc là DHT.
- Cuối cùng là Tìm hiểu về Chord, một loại mạng của DHT.
### 1. Mạng Peer-to-Peer
- Mạng Peer-To-Peer( mạng ngang hàng) là một mạng được tạo nên bởi các máy tính liên kết với nhau, vai trò của mỗi máy tính là như nhau, mỗi máy tính là một phần và duy trì sự tồn tại của hệ thống mạng. 
- Các máy tính thường xuyên liên lạc với nhau để ổn định mạng và chia sẻ dữ liệu với nhau, mỗi máy tính gửi thông tin lên toàn mạng để cho biết sự có mặt của nó.
- Một mạng ngang hàng là một mạng mà tất cả các máy tham gia đều bình đẳng (gọi là Peer), một node trong mạng vừa đóng vai trò là client để tìm kiếm và lấy thông tin từ các node khác, vừa là server có nhiệm vụ lưu trữ dữ liệu và truyền dữ liệu cho các node khác trong mạng, và một điều đặc biệt là tất cả các máy tính trong một mạng đều tham gia đóng góp tài nguyên như: Băng thông, lưu trữ cũng như khả năng tính toán...
- Mạng ngang hàng có rất nhiều ứng dụng, đặc biệt là Hệ thống chia sẻ tệp tin về âm thanh, hình ảnh, dữ liệu...
điển hình như hệ thống mạng Napster, Gnutella hay Freenet....     
### **Như vậy, so với mạng truyền thống (Client/Server) thì mạng peer-to-peer có đặc điểm nổi bật là gì** ????   
<<<<<<< HEAD
<img src="../Bao cao Chord/bao_cao/sosanh.png">  
=======
<img src="./bao_cao/sosanh.png">  
>>>>>>> e76c0117c0222fd9593338ee1e95279866ba37d9
Hình ảnh trên nói về sự so sánh hai cấu trúc mạng Client/Server và Peer-To-Peer.

### **Từ hình ảnh trên ta có thể đưa ra các đặc điểm nổi của mạng Peer-To-Peer so với mạng client/Server**:    

- Việc các máy tính đều tham gia vào việc lưu trữ, tính toán, hay truyền dữ liệu...tránh việc hệ thống hay một máy nào đó bị quá tải, cũng như tốc độ truyền cũng sẽ nhanh hơn. Còn đối với bên Client/Server, việc lưu dữ liệu trên một Server sẽ khiến Server bị quá tải, hiệu suất hoạt động tăng, khả năng tính toán giảm do phải xử lý yêu cầu nhiều, cũng như khả năng truyền tin giảm khi mà số lượng Client tăng lên.   
- Việc mà các máy trong mạng Peer-To-Peer vừa đóng vai trò là client, vừa đóng vai trò là Server, tức là một máy tính có thể được chia sẻ dữ liệu của các máy khác, cũng như có thể tham gia chia sẻ dữ liệu của mình cho các máy khác trong mạng làm cho dữ liệu tăng lên số lượng bản sao, giúp cho việc tìm kiếm nhanh chóng, và khả năng mất dữ liệu là cực kỳ thấp. Còn bên Clien/Server, tất cả dữ liệu được lưu trữ trên một Server, làm cho client phụ thuộc quá nhiều vào Server, khi mà Server gặp sự cố (có thể bị lỗi, hay bị hỏng....) thì toàn hệ thống mạng ngừng hoạt động, hay việc dữ liệu trên Server bị lỗi hoặc bị thay đổi, thì việc tìm lại dữ liệu ban đầu là rất khó.
- Mạng Peer-To-Peer có tính chất phân tán, tức là dữ liệu được phân tán trên các node mạng khác nhau, điều này giúp cho việc hệ thống vẫn hoạt động bình thường khi một node trong mạng bị lỗi hoặc hỏng hoặc là có node tham gia, hoặc là rời khỏi hệ thống .Với cấu trúc mạng Client/Server thì khi máy chủ bị sự cố thì toàn bộ hệ thống sẽ ngừng hoạt động.    
Tuy nhiên đối với mạng Peer-To-Peer, có một số hệ thống vừa sử dụng cấu trúc mạng Peer-To-Peer, kết hợp với Clien/Server (đây được gọi là mạng Hybrid).   
Trên hình ảnh cho thấy trong cấu trúc mạng Peer-To-Peer, ta thấy rằng được chia thành hai loại chính là: **mạng không có cấu trúc** và **mạng có cấu trúc**, Vậy đặc điểm và cách thức hoạt động của chúng ra sao ?? 
### a. Mạng Peer-To-Peer không có cấu trúc
Một mạng Peer-To-Peer không có cấu trúc là mạng mà các liên kết giữa node trong mạng được thiết lập một cách ngẫu nhiên( không có quy luật).   
Những mạng này có thể dễ dàng xây dựng vì khi một máy muốn tham gia, nó có thể lấy các liên kết có sẵn của một máy khác trong mạng, và dần dần thêm các liên kết mới cho nó.    
khi một máy muốn tìm kiếm dữ liệu, yêu cầu tìm kiếm sẽ được truyền trên cả mạng (truyền trên tất cả các liên kết mà nó nó) để tìm ra càng nhiều máy chia sẻ càng tốt.    
 Một số hệ thống phổ biến có sử dụng cấu trúc mạng ngang hàng không có cấu trúc như: Napster, Gnutella, ....   

Dựa vào các đặc điểm của một mạng ngang hàng không có cấu trúc, ta dễ dàng đưa ra các nhược điểm của chúng:   
- Khi tìm kiếm dữ liệu, việc một máy phải gửi yêu cầu cho tất cả các máy mà nó liên kết làm cho chi phí băng thông gửi tới các máy là cao.
- Vì mạng là là không có cấu trúc, nên nó không đảm bảo rằng việc tìm kiếm dữ liệu là thành công, vì nó không có mối tương quan giữa dữ liệu và máy.
- Đối với dữ liệu được lưu bản sao trên nhiều máy, thì việc tìm kiếm dữ liệu thành công là khá cao, ngược lại, nếu dữ liệu chỉ lưu trên một vài máy, thì xác suất tìm thấy là rất nhỏ. hoặc có thể là không tìm thấy.
### b. Mạng Peer-To-Peer có cấu trúc
Mạng Peer-To-Peer có cấu trúc khắc phục được các nhược điểm của mạng Peer-To-Peer không có cấu trúc bằng các sử dụng Hệ thống DHT( Distributed Hash Table ). Hệ thống này định nghĩa các liên kết giữa các node trong mạng theo một quy luật(thuật toán) nào đó, đồng thời xác định mỗi node trong mạng sẽ chịu trách nhiệm  đối với một phần dữ liệu có trong mạng.
- trong mạng ngang hàng có cấu trúc, dữ liệu được phân bố một cách hợp lý để không có máy nào lưu trữ dữ liệu quá nhiều hay quá ít, tránh việc quá tải.
- Do mạng là có cấu trúc, nên việc một máy truyền thông điệp tới các máy khác để duy trì mạng được giảm đáng kể.   
- Đối với mạng không có cấu trúc, việc tìm kiếm dữ liệu nó phải Broadcast để tìm kiếm dữ liệu, thì đối với mạng có cấu trúc, nó chỉ cần gửi thông điệp tới một số máy cần thiết. Điều này làm cho thời gian tìm kiếm nhanh hơn, và Chi phí tìm kiếm giảm đáng kể so với mạng không có cấu trúc.
    
Như vậy phần trên đã nói về mạng Peer-To-Peer có cấu trúc và không có cấu trúc, các đặc điểm của hai loại cấu trúc trên, và ta cũng nhận thấy rằng, mạng Peer-To-Peer có cấu trúc có các đặc điểm giải quyết được các nhược điểm của mạng Peer-To-Peer không có cấu trúc, đặc biệt là hệ thống DHT. Vậy thì DHT là gì? đặc điểm của DHT ra sao? và cách thức hoạt động như thế nào? 

### 2. Distributed Hash Table
Trước khi đưa ra khái niệm và đặc điểm cũng như cách thức hoạt động của DHT, ta sẽ giới thiệu các hệ thống có cấu trúc là mạng Peer-To-Peer không có cấu trúc và nhược điểm của chúng:  
- **Hệ thống Napster**:  Napster sử dụng một Server trung tâm, mỗi khi một node tham gia vào hệ thống mạng, node đó cần phải gửi danh sách dữ liệu cho Server, mỗi khi mà một node muốn tìm kiếm, node đó sẽ gửi một thông điệp yêu cầu đến Server, Server có trách nhiệm xử lý truy vấn, và tìm ra node chứa dữ liệu cần tìm, rồi gửi lại địa chỉ IP của node chứa dữ liệu cho node cần tìm, sau khi biết được địa chỉ IP, thì các node sẽ giao tiếp trực tiếp với nhau để chia sẻ dữ liệu cho nhau mà không cần thông qua Server. Có một nhược điểm trong hệ thống này là thành phần trung tâm dễ bị tấn công hoặc có thể bị lỗi... thì hệ thống ngừng hoạt động.  
- **Hệ thống Gnutella**: đây là mô hình mạng Peer-To-Peer thuần túy và không có cấu trúc. không còn máy chủ tập trung như Napster, nó khắc phục được điểm yếu của Napster, nhưng nó lại sử dụng cơ chế Flooding trong vấn đề tìm kiếm, tức là khi tìm kiếm, nó sẽ gửi yêu cầu cho **tất cả** các node hàng xóm của nó, truy vấn sau đó sẽ được chuyển dần qua các node khác và tới được node có chứa dữ liệu cần tìm. Như vậy chi phí tìm kiếm là rất lớn, không hiệu quả so với Napster.  

Để khắc phục được các nhược điểm trên của hệ thống mạng không có cấu trúc, ta sử dụng Hệ thống mạng có cấu trúc, đặc biệt là hệ thống DHT.
- DHT(Distributed Hash Table) là một phương pháp, một cách để tổ chức dữ liệu, cung cấp dịch tìm kiếm dữ liệu tương tự như một bảng băm, tức là cung cấp ánh xạ từ key -> value. Và bất kỳ node nào trong mạng cũng có thể lấy được giá trị từ một key cho trước.
- DHT đảm bảo được tính phân tán (giống như Gnutella) và hiệu quả trong việc tìm kiếm dữ liệu (giống như Napster) bằng cách sử dụng cơ chế định tuyến dựa trên khóa có cấu trúc. 

### 2.1 Đặc điểm của DHT
DHT nhấn mạnh vào các đặc điểm sau:    
- **Tính phân tán**: nó là một mạng Peer-To-Peer thuần túy, tức là không có thành phần máy chủ, chỉ có các node tham gia hệ thống, không có node nào quan trọng hơn node nào, như nhau đối với mọi node.  
- **Khả năng mở rộng**: Hệ thống vẫn có thể hoạt động hiệu quả khi số lượng node tăng lên (lên tới hàng triệu node). tức là việc node tham gia (hoặc rời khỏi) hệ thống tăng lên thì hệ thống hoạt động bình thường.
- **khả năng chịu lỗi**: tức là một hệ thống xảy ra các sự kiện các node tham ra( hoặc rời khỏi) hệ thống hoặc bị lỗi liên tục, thì hệ thống vẫn hoạt động ổn định. 

### 2.2 cấu trúc của DHT
Cấu trúc DHT được chia thành ba thành phần chính:   
    + Không gian khóa : tập các giá trị khóa m-bit  
    + Cách thức phân chia không gian khóa: cách thức chia không gian khóa cho từng node tham gia trong mạng.   
    + mạng Overlay: cách kết nối các node trong mạng với nhau, cho phép một node có thể tìm kiếm được một node chứa giá trị cần tìm.  

***Cách thức sử dụng DHT cho việc **lưu trữ** và **tìm kiếm** dữ liệu trong mạng***: 
- Giả sử không gian khóa là tập các giá trị m-bit. Để lưu trữ *data* của một file tên là *filename* vào trong DHT, nó thực hiện băm *filename* để tạo ra một khóa k có độ dài là m-bit, sau đó gửi 1 thông điệp *put(k,data)* tới một node bất kỳ trong DHT. Thông điệp được chuyền từ node này tới node khác thông qua *mạng Overlay* cho đến khi tìm được node chịu trách nhiệm quản lý khóa k được xác định nhờ cách thức phân chia khóa, khi đó cặp (k,data) được lưu trữ.   
- Mọi node có thể tìm kiếm nội dung của một *filename* bằng cách băm *filename* để sinh ra khóa k, rồi thực hiện get(k) tới một node bất kỳ trong DHT để tìm dữ liệu, thông qua mạng *Overlay*, thông điệp được gửi từ node này tới node khác cho đến khi tìm thấy node chịu trách nhiệm quản lý khóa k, khi đó dữ liệu *data* được trả về.    
### **Cách thức phân chia không gian khóa** : Hầu hết DHT sử dụng hàm băm nhất quán (Consistent hash) để ánh xạ khóa tới node. nó sử dụng một hàm δ(k1, k2) để định nghĩa khái niệm trừu tượng về khoảng cách giữa k1,k2. Mỗi node được gán một khóa gọi là định danh-ID. một node có ID là i sở hữu tất cả các khóa mà ID i gần với các khóa này được ước lượng theo hàm δ.    
vidu: Chord DHT coi các khóa là các điểm trên vòng tròn, khoảng cách δ(k1, k2) được tính theo chiều kim đồng hồ xung quanh từ khóa k1 đến k2. Do đó không gian đường tròn được chia thành các cung liên tiếp mà điểm cuối của cung là ID của node. nếu ID i1,i2 nằm liền kề nhau, thì tất cả các khóa nằm giữa i1 và i2 được sở hữu bởi node có định danh ID i2.   
**Consistent Hashing** có một đặc điểm quan trọng là việc **gỡ bỏ** hay **thêm** vào các node chỉ làm thay đổi tập các khóa sở hữu bởi các node có ID liền kề, các node khác không ảnh hưởng. Tính chất này trái với hàm băm truyền thống mà trong đó, việc thêm hoặc bớt khối dữ liệu dẫn đến việc phải ánh xạ lại gần như toàn bộ không gian khóa.
### **Mạng Overlay**: Mỗi một node duy trì một tập các liên kết với các node khác( gọi là ndoe hàng xóm hoặc có thể gọi là bảng định tuyến-finger table). các liên kết này tạo ra một mạng Overlay. 1 node kết nạp các node khác vào làm hàng xóm của nó dựa trên một cấu trúc nào đó, gọi là Network topology.   
- Tất cả DHT topology đều có chung một thuộc tính quan trọng là: ứng với một khóa k bất kỳ, một node bất kỳ có thể định tuyến truyền thông điệp tới node quản lý khóa k thông qua việc sử dụng thuật toán tham lam: ở mỗi bước, một node có thể định tuyến thông điệp tới hàng xóm của nó mà ID của node hàng xóm đó gần nhất với khóa k. khi không có hàng xóm nào như thế, thì chúng ta đã đến node quản lý khóa k. kiểu định tuyến này được gọi là **định tuyến dựa trên khóa k**.  
- *có 2 yếu tố quan trọng trong một topology* là: chi phí tìm kiếm và số lượng node hàng xóm của một node. để chi phí tìm kiếm tối đa là nhỏ nhất, thì đòi hỏi số lượng hàng xóm cao, và ngược lại, số lượng hàng xóm tối đa thấp nhất, thì việc tìm kiếm dẫn đến chi phí tăng lên.    
 Vậy thì số lượng node hàng xóm và chi phí tìm kiếm tối đa nhỏ nhất là bao nhiêu mới hợp lý, phù hợp với hệ thống??    
Dưới đây là một số lựa chọn trong việc điều chỉnh hai yếu tố trên:     
    - Degree O(1), route length O(logn)  
    - Degree O(logn), route length O  (logn/loglogn)  
    - Degree O(logn), route length O(logn)  
    - Degree O(n1/2), route length O(1)     
*Trong đó Degree: số hàng xóm tối đa; Route length: số hop tối đa trên đường định tuyến; n: số node tham gia trong hệ thống.*    
Với sự lựa chọn là *Degree O(logn), route length O(logn)* là phổ biến khi mà một hệ thống cần cân bằng cả hai yếu tố trên.        

#### **Như vậy các nội dung trên đã giới thiệu qua về DHT, các đặc điểm, tính chất cũng như cách thức hoạt động của nó, nội dung tiếp theo là giới thiệu về Chord, một loại mạng phổ biến của DHT.**    
### 3. Chord   
#### 3.1 giới thiệu về Chord
Chord là một giao thức sử dụng DHT nhằm mục đích tổ chức, tìm kiếm dữ liệu một cách phân tán tốt nhất.   
Chord có những đặc điểm ưu thế của mình, có hai đặc điểm nổi bật đó là khả năng tìm kiếm dữ liệu nhanh và cân bằng tải giữa các node. Và có một đặc điểm quan trọng là sự phân phối các khóa tới các node trong mạng là tương đối đồng đều, đây là một hệ quả của việc sử dụng kỹ thuật *consistent hashing* trong việc cấp khóa cho các node.  
Chord coi các khóa là các điểm trên vòng tròn, không gian khóa được chia thành nhiều cung tròn mà điểm cuối cùng là các định danh ID của các node. Mỗi node lưu trữ thông tin định tuyến tới các node khác trong bảng định tuyến gọi là finger table.    
<img src="../Bao cao Chord/vongchord.png">   
- Giao thức Chord hỗ trợ một hoạt động duy nhất: khi đưa ra một key, nó sẽ ánh xạ key đó vào một node (sử dụng Consistent hashing). Node đó sẽ lưu giá trị (value) cùng với key đó. Chord sử dụng kỹ thuật *consistent hashing* để cấp key cho các node, không những vậy, *consistent hashing* còn dùng để cân bằng tải các node mạng, mỗi node sẽ nhận được số lượng key gần bằng nhau, hay chuyển một số lượng key sang node khác khi có một node tham gia hoặc rời khỏi hệ thống.       
#### 3.2 Mô hình mạng Chord   
Chord được mô tả dưới dạng một vòng tròn và có không định danh m-bit, mạng Chord sẽ có thể chứa tối đa 2<sup>m</sup> node. Mỗi một node sẽ được gán một định danh là id, và các id được sắp xếp thành một vòng tròn theo thứ tự tăng dần theo chiều kim đồng hồ.   
Chord sử dụng hàm băm để sinh khóa(id), đầu ra là giá trị có độ dài m-bit.    
trong một node cụ thể, node mà nó trỏ tiếp theo có id **lớn hơn** gọi  là *Successor(id)*, Node mà nó trỏ có id **nhỏ hơn** gọi là *Predcessor(id)* . Và tất cả đều được lưu vào một node trỏ tới chúng.    
Khi mạng Chord được thiết lập, một hệ thống gồm n node, trong đó mỗi node lưu trữ thông tin của O(logn) node khác trong mạng, và việc tìm kiếm các node khác nó cũng chỉ cần gửi thông điệp tối đa tới O(logn) các node hàng xóm của nó.   
Khi một hệ thống có tần suất các node tham gia/rời khỏi mạng cao, thì việc cập nhật lại thông tin bảng định tuyến là rất cần thiết, khi một node tham gia vào mạng, nó cũng chỉ cần không quá O(logn) thông điệp gửi tới các node khác để ổn định thông tin định tuyến.   
Chord ánh xạ các khóa vào các node, thường sẽ là cặp key/value. value có thể là một message, một file, hoặc một mục dữ liệu...
Một node sẽ chịu trách nhiệm chứa cặp key/value khi mà node đó có định danh id nhỏ nhất lớn hơn k, khi đó node đó được gọi là Successor(k).   
#### 3.3 các đặc điểm của Chord  
Chord có những đặc điểm sau đây: 
- **Cân bằng tải(Load Balance)**: Chord sử dụng bảng băm phân tán ,việc phân bổ khóa trên các node dựa trên thuật toán **consistent hashing** sẽ giải quyết được vấn đề cân bằng tải và khi đó một node sẽ không chứa quá nhiều số lượng key.   
- **Phân quyền (Decentralization )**: Chord là phân tán hoàn toàn, không có node nào quan trọng hơn node nào, các node đều có chức năng giống nhau, và bình đẳng giữa các node trong mạng.    
- **khả năng mở rộng (Scalability)**: Việc mà các node tham gia vào mạng tăng lên, nhưng vẫn đảm bảo rằng hệ thống vẫn hoạt động bình thường và chi phí tìm kiếm thì tăng theo O(logn) (n: số node trong mạng).  
- **Tính sẵn sàng(Availability)**: Mỗi một node sẽ tự điều chỉnh bảng finger table của chính nó khi có một node tham gia hoặc rời khỏi hệ thống. Hay nói cách khác, trong mạng Chord quá trình duy trì sự tồn tại của mạng diễn ra hoàn toàn tự động.   
- **Linh hoạt(Flexible naming)**: Chord không ràng cuộc về cấu trúc của key. không gian khóa là bằng phẳng, linh hoạt trong việc gán một cái tên tìm kiếm cho một key.   
### 3.4 Hàm băm nhất quán (Consistent Hashing)  
- Consistent hashing thực hiện gán mỗi node và khóa một định danh m bit sử dụng hàm băm SHA-1.
- Định danh của mỗi node là giá trị băm địa chỉ IP của node đó.  
- Định danh của mỗi khóa là giá trị băm của tên hoặc nội dung của dữ liệu.  
- Consistent hashing sẽ gán khóa k cho node có định danh bằng hoặc lớn hơn đứng sau khóa k, khi đó node đó gọi là successor(k).   
#### 3.4 Finger table
trong Chord thì mỗi node sẽ có một bảng Finger table để lưu giữ thông tin của các node hàng xóm của nó. một Finger table thì lưu giữ thông tin O(logN) node khác.   
Trong một Finger table lưu trữ: chỉ mục, các id và các hàng xóm chịu trách nhiệm quản lý các id đó.
<img src="../Bao cao Chord/finger1.png">   
- Tại mục thứ i (bắt đầu từ 0) trong finger table của một node p có lưu trữ giá trị id là: p + 2<sup>i-1</sup> và Successor của nó là successor(p + 2<sup>i</sup>).   
Khi một node tham gia vào mạng, việc nó phải làm là sẽ tìm một định danh id và báo cho các node bên cạnh biết có sự tham gia của nó nhờ sử dụng một thuật toán là stabilization. các node Successor và Predecessor sẽ phải cập nhật thông tin về node mới tham gia vào mạng. Một phần khóa của Successor sẽ được chuyển sang node mới. Node mới tham gia cần phải tạo ra một bảng finger table và cập nhật các giá trị vào trong bảng, và đông thời các node trong mạng cần thường xuyên cập nhật lại thông tin về node hàng xóm cũng như cần phải cập nhật các mục trong bảng Finger table. 
Khi một node rời khỏi mạng, nó cần phải thông báo cho các node bên cạnh để ổn định lại mạng, khi đó phần khóa của node rời khỏi mạng sẽ được chuyển sang cho Successor của nó. 
#### 3.5 Tìm kiếm trong Chord
Khi một node cần tìm dữ liệu của khóa k, thì node đó phải tìm kiếm node chứa khóa k đó, nếu như node ở xa vị trí so với node lưu khóa k, nó có thể dựa trên các node hàng xóm của nó thông quan bảng định tuyến (finger table) để định tuyến, từ đó dần dần nó sẽ tìm ra node chứa khóa k đó.   
<img src="../Bao cao Chord/find.png">  
Giả sử rằng trên vòng tròn Chord có các node như trong hình vẽ, N8 cần tìm node chứa khóa k có giá trị là 54, Đầu tiên nó kiểm tra khóa k có nằm giữa nó với Successor không, nếu nằm giữa, thì Successor của N8 là node chứa dữ liệu của khóa k và là node nó cần tìm, Nếu không nó sẽ tìm trong bảng Finger table của chính nó mục thứ i trong sao cho giá trị : m = N8 + 2<sup>i</sup> <= k < N8 + 2<sup>i+1</sup>, trong hình trên node thỏa mãn là N42 (i=5) là successor(m), N8 sẽ gửi một yêu cầu tìm node chứa khóa k cho N42, khi đó N42 tiến hành giống như N8, và node tiếp theo sẽ là Node N51, và đến node 51, nó tìm ra node chịu trách nhiệm chứa khóa k là N56, và khi đó N56 sẽ được trả về cho N8.   











