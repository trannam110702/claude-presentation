# Kịch bản thuyết trình: Agent Skills hay MCP trong kỷ nguyên Claude Code

> Ghi chú: Phần in nghiêng *(…)* là chỉ dẫn sân khấu — tạm dừng, chuyển slide, ánh mắt, v.v. Đừng đọc to những phần này.

---

## Slide 1 — Mở đầu

*(Bước ra giữa, mỉm cười, nhìn lướt khán phòng khoảng 2 giây trước khi nói)*

Xin chào cả nhà. Cảm ơn mọi người đã dành thời gian cho mình sáng/chiều hôm nay.

Trong khoảng 10–15 phút tới, mình muốn cùng cả nhà trả lời một câu hỏi mà gần đây mình nghe rất nhiều — cả trên Twitter, trong các nhóm dev, và ngay cả trong các buổi cà phê nội bộ của tụi mình. Câu hỏi đó là: *“Skills đang phủ sóng khắp nơi — vậy nó có khai tử MCP không?”*

Câu trả lời ngắn của mình là: **có thể**. Nhưng để trả lời cho đàng hoàng, trước hết mình muốn nhìn nhận MCP một cách công bằng đã. Sau đó tụi mình sẽ nói về Skills, và cuối cùng là chỗ hai cái đó đụng nhau — và chỗ chúng *không* đụng nhau.

*(Tạm dừng 1 nhịp, bấm chuyển slide)*

---

## Slide 2 — Ôn lại về MCP

Đầu tiên, ôn lại nhanh một chút về MCP cho những ai chưa quen.

Hình dung thế này: chúng ta có một agent — có thể là Claude Code chạy trên laptop, có thể là một microservice đâu đó trên cloud. Người dùng gửi prompt vào, agent gọi LLM để xử lý.

Vấn đề là gì? *(Bước nhẹ sang trái, chỉ vào cột bên trái)* LLM chỉ biết những thứ nó đã đọc trên Internet. Nó **không biết gì** về ticket của tụi mình, về wiki nội bộ, về Google Docs, về dữ liệu khách hàng. Mà nó cũng không thể *làm* được gì cả — nó chỉ trả về văn bản.

Nhưng tụi mình lại muốn agent **tạo ra tác động thật** ngoài thế giới: mở ticket, deploy code, gửi email, sửa file. Đúng không?

*(Bước sang phải, chỉ sang khối MCP server màu xanh đậm)*

Và đó chính là chỗ MCP server xuất hiện. Nó đứng giữa agent và mớ API hỗn độn bên ngoài. Nó thực thi tool call, cung cấp resource, và quan trọng nhất — nó cho agent một **giao diện chuẩn hoá**, cắm-là-chạy. Bạn chỉ cần trỏ agent tới MCP server, và server sẽ tự quảng bá khả năng của mình. Rất mạnh, rất linh hoạt.

*(Tạm dừng. Chuyển slide.)*

---

## Slide 3 — 9–12 tháng qua MCP đã thay đổi gì

Từ video trước của mình tới nay, MCP cũng đã thay đổi kha khá. Mình muốn highlight 3 thứ đáng chú ý nhất.

**Thứ nhất** *(chỉ vào ô đầu tiên)* — và cái này khá thú vị — là **resources gần như bị bỏ rơi**. Cả nhà thử ra ngoài tìm các MCP server đang chạy thật xem, đọc tài liệu của họ — gần như không ai dùng resource API nữa. Cộng đồng đã âm thầm chọn dùng tool call cho mọi thứ. Một truy vấn resource giờ chỉ là một tool call. Đây là một bài học rất hay về việc thiết kế API: đôi khi mình nghĩ ra hai khái niệm, nhưng thị trường chỉ chọn một.

**Thứ hai** là **Streamable HTTP**. Trước đây MCP dùng server-sent events, gây đau đầu cho những ai muốn deploy server lên cloud. Streamable HTTP gọn hơn, dễ deploy hơn, dễ debug hơn. Đây là một nâng cấp lớn về mặt hạ tầng.

**Và thứ ba** là **OAuth 2.1**. Cái này quan trọng. Trước đây auth là điểm đau lớn — bạn phải tự nhét secret vào biến môi trường, tự lo token. Bây giờ user thấy đúng cái pop-up OAuth quen thuộc — kiểu như "Ứng dụng này muốn truy cập GitHub của bạn, đồng ý không?". Một MCP server trên cloud có thể lo việc auth thay cho agent local. Trải nghiệm trở nên rất tự nhiên.

*(Tạm dừng. Hạ giọng một chút trước khi chuyển slide.)*

Nhưng — và đây là một chữ "nhưng" quan trọng — OAuth không phải lúc nào cũng đẹp như vậy. Cùng xem slide tiếp theo.

---

## Slide 4 — OAuth: tuyệt cho laptop, vướng cho microservice

*(Chỉ vào ô bên trái)*

Với agent chạy trên laptop — kiểu như Claude Code — OAuth hoạt động cực kỳ ngon. Người dùng đang ngồi trước máy, pop-up bật lên, bấm Approve, xong. Agent nhận token, mọi thứ chạy mượt mà. Đây là kịch bản mà 90% chúng ta đang gặp hôm nay.

*(Chỉ sang ô bên phải)*

Nhưng giả sử bạn viết một **microservice agentic** chạy headless trên cloud, không có ai ngồi sẵn để bấm "Approve" thì sao? Pop-up OAuth lúc này hoàn toàn không hợp. Đây là một vấn đề mở mà cộng đồng đang giải quyết. Có hứa hẹn về giải pháp tốt hơn, nhưng tới thời điểm hiện tại thì vẫn còn vướng. Đây là chỗ cả nhà nên để mắt theo dõi.

*(Tạm dừng — đây là chỗ chuyển ý quan trọng. Hít một hơi.)*

Ok, đó là chuyện của MCP. Bây giờ — câu hỏi to nhất của hôm nay: **Skills là gì, và liệu nó có thay được MCP không?**

---

## Slide 5 — Skill là gì?

Trước hết, với những ai chưa quen với Skills — mình muốn cả nhà hiểu thật rõ Skill thực sự là cái gì, vì mình thấy nhiều người đang nghĩ nó phức tạp hơn thực tế.

**Bản chất, một Skill chỉ là một file văn bản.** Đúng vậy — một file `skill.md` nằm trong một thư mục nào đó mà agent có thể đọc được. Trong file đó là một prompt mở rộng — chứa quy trình, lời khuyên, các điều kiện để xử lý một tác vụ cụ thể.

Ví dụ: tìm việc cho mình, làm vườn, sửa file Excel — những việc mà LLM thuần có thể làm sơ sơ, nhưng chưa đủ giỏi. Skill là cái nơi mình **mã hoá phần "làm cho thật giỏi"** đó.

Và Skill không chỉ có file markdown. Nó có thể tham chiếu các thư mục con: một thư mục `/resources` chứa dữ liệu tham khảo tĩnh — ví dụ thông tin khí hậu, mô hình 3D của cây cối nếu bạn làm skill landscaping. Hoặc một thư mục `/scripts` chứa bash, Python — những script làm được những việc model một mình rất khó làm, ví dụ sửa đúng một ô Excel cụ thể qua API.

Một điều quan trọng nữa: **Skills không host trên cloud**. Nó là một cây thư mục trên đĩa. Bạn ship nó qua GitHub, đặt nó vào file system nơi agent có thể thấy. Vậy thôi. Đơn giản đến mức… ngạc nhiên.

*(Tạm dừng. Bấm chuyển slide.)*

---

## Slide 6 — Skills và MCP: chỗ chồng lấn và chỗ khác biệt

Tới đây cả nhà chắc bắt đầu thấy *vì sao* lại có người hỏi câu "Skills có giết MCP không". Hai cái nhìn xa thì rất giống nhau. Cả hai đều có resource, đều có cái gì đó kiểu "hành động". Nhưng nhìn gần thì chúng *khác nhau* khá rõ.

*(Chỉ vào dòng "Resources" trong bảng)*

**Về Resources** — MCP thiên về dữ liệu **thời gian thực**: khách hàng, ticket, dữ liệu chảy qua Kafka. Còn Skills thiên về dữ liệu **tham khảo tĩnh** — những thứ có khi đã nằm sẵn đâu đó trong weights của model, nhưng tụi mình muốn kéo nó ra phía trước, nhấn mạnh "cái này quan trọng đó".

*(Chỉ xuống dòng "Hành động")*

**Về hành động** — MCP hướng ra **mạng**: gọi API, đụng tới dịch vụ cloud, thường cần auth. Skills hướng vào **local**: script bash, Python, tác động lên file system trên máy bạn.

*(Chỉ xuống dòng "Định hướng")*

Và đó dẫn tới sự khác biệt lớn nhất về **định hướng**: MCP nhìn ra ngoài cloud. Skills nhìn vào trong laptop. Hai góc nhìn rất khác nhau, dù trông bề ngoài có vẻ chồng lấn.

*(Tạm dừng. Chuyển slide.)*

---

## Slide 7 — Lập luận "Skills thay được MCP"

Vậy thì lập luận "Skills thay được MCP" đến từ đâu? Mình muốn nói thẳng — **lập luận đó không phải vô lý**. Hãy nghe thử.

*(Chỉ vào ô bên trái)*

Tưởng tượng một coding agent local. Bạn viết một Skill, trong đó có một script. Script đó shell out gọi một CLI nào đó — ví dụ `gh`, `aws`, `gcloud`. CLI đó tự lo auth, tự lo token. Và đột nhiên… nó nhìn rất giống một MCP tool call, đúng không?

Với coding agent local — case phổ biến nhất hiện nay — đây là một phương án thay thế **hoàn toàn hợp lý**. Mình hiểu vì sao có người đề xuất bỏ MCP và chỉ dùng Skills.

*(Chuyển ánh mắt và giọng sang phải)*

**Nhưng** — và đây là chữ "nhưng" quan trọng nhất của cả buổi
MCP thực ra chưa “chết”, mà ngược lại nó vẫn có những nền tảng rất quan trọng mà Skills hiện chưa làm tốt, đặc biệt là về authentication. MCP được thiết kế với OAuth tích hợp sẵn, cho phép người dùng đăng nhập trực tiếp vào các dịch vụ như GitHub hay Jira và sử dụng token theo đúng quyền cá nhân, thay vì phải dùng API key chung như trong Skills — một cách làm kém an toàn và khó áp dụng trong môi trường doanh nghiệp. Ngoài ra, MCP không phụ thuộc vào việc quản lý secrets toàn cục và phù hợp hơn cho các hệ thống tích hợp SaaS thực tế. Về mặt vận hành, MCP hướng tới việc để AI tự khám phá và sử dụng tools một cách linh hoạt, trong khi Skills lại phụ thuộc nhiều vào các hướng dẫn dạng step-by-step giống tài liệu, vốn khó mở rộng và nhanh lỗi thời khi hệ thống lớn lên. Vì vậy, dù MCP hiện có vấn đề về “phình to context”, nhưng đây là vấn đề có thể giải quyết, và về lâu dài MCP vẫn là nền tảng tốt hơn cho các hệ thống agent quy mô lớn.

Lúc này, Skills, scripts, CLIs… **không khớp** với hình hài của bài toán. Cái bạn cần là **giao diện chuẩn hoá** mà MCP cung cấp. Bạn vẫn rất cần MCP.

Vì vậy câu trả lời của mình cho câu hỏi đầu buổi là: *không, Skills không thay thế MCP — ít nhất là chưa.*

*(Tạm dừng dài hơn một chút. Đây là kết luận.)*

---

## Slide 8 — Những điểm cần nhớ

Để chốt lại trước khi mình mở phần Q&A — bốn ý chính cả nhà nên mang về.

**Một**, *(chỉ ô đầu tiên)* Skills không thay thế MCP. Có chồng lấn thật, nhưng chúng giải quyết bài toán ở các tầng khác nhau.

**Hai**, MCP vẫn rất cần — đặc biệt nếu công ty mình bắt đầu xây những agent headless, microservice của riêng tụi mình.

**Ba**, Skills cũng rất cần — đặc biệt cho những quy trình local, những best practice của team mà tụi mình muốn agent luôn tuân theo. Đây thực ra là chỗ tụi mình có thể bắt đầu **ngay hôm nay**: viết vài SKILL.md để chuẩn hoá cách mà Claude Code làm việc trên repo của tụi mình.

**Và bốn** — cái này hơi dài hạn — hãy theo dõi sự **hội tụ**. Đã có Python API cho phép agent tự viết của mình thực thi Skills. Ý tưởng từ thế giới coding agent đang lan ra ngoài. Mình nghĩ trong 6–12 tháng tới ranh giới giữa hai cái sẽ mờ đi nữa.

*(Hít một hơi, mỉm cười)*

Tóm lại một câu: **Bạn cần biết, và cần dùng, cả hai.** Đây có lẽ là giai đoạn tiến hoá nhanh nhất mà mình từng thấy trong nghề. Vui và mệt cùng lúc — nhưng chủ yếu là vui.

*(Tạm dừng 2 nhịp.)*

Cảm ơn cả nhà đã lắng nghe. Mình rất muốn nghe câu hỏi và phản biện từ mọi người — đặc biệt là nếu ai đó trong phòng đang xây microservice agentic, hoặc đang viết Skill cho team của mình. Xin mời.

*(Bước lùi nửa bước, mở lòng bàn tay về phía khán phòng để mời câu hỏi.)*

---

## Một vài mẹo nhỏ khi trình bày

- **Tốc độ**: chậm hơn bạn nghĩ. Mỗi slide khoảng 1.5–2 phút là đẹp.
- **Tạm dừng**: mỗi khi chuyển slide, im lặng 1 nhịp — đừng lấp khoảng lặng bằng "ờ", "à".
- **Ánh mắt**: chia khán phòng thành 3 vùng (trái, giữa, phải), mỗi câu nhìn một vùng.
- **Tay**: dùng tay để chỉ vào slide khi nói "ô này", "cột bên trái" — đừng để tay cứng.
- **Câu chốt mỗi slide**: nhấn nhẹ giọng vào câu cuối để khán giả biết bạn sắp chuyển ý.
- **Q&A**: nếu không biết câu trả lời, hãy nói thẳng "Câu này hay, mình chưa có câu trả lời chắc chắn — để mình tìm hiểu rồi gửi lại sau."
