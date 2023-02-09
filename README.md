# tw-custom-chat-overlay
<p>
    สวัสดีครับ จารย์กานต์ Sir Lucian ครับผม
</p>
<p>
    สำหรับโปรเจคนี้ก็ได้พัฒนามาซักพักใหญ่ๆแล้วล่ะครับ แค่ยังไม่ได้มีเวลาว่างมาเขียน Documentation หรือ Blogpost
    จนกระทั่งวันนี้! เราจะมานำเสนอนวัตกรรมเขียนเอง งมเอง Suffer เอง และเฮฮาสังสรรค์ไปด้วยกันกับสิ่งที่เรียกว่า Twitch
    Chat Overlay กันนะครับ
</p>
<p>
    สำหรับคนที่ยังไม่คุ้นชินกับเหล่าสตรีมเมอร์ ไม่ว่าจะเป็น Platform ไหนนะครับ สตรีมเมอร์มักจะเพิ่ม Live Chat
    ลงไปในหน้าสตรีมของเขา ซึ่งช่องที่เขาใส่ใจรายละเอียดหรืออยากให้มันเข้ากับธีมช่อง
    เขาก็จะดัดแปลงช่องการแสดงแชทนี้ให้เข้าธีมครับ ไม่ว่าจะเป็นการปรับรูปลักษณ์ ลูกเล่นต่างๆ ให้เหมาะสม
    บางคนก็แนวน่ารักบางคนก็ Edgy หรือสายมีมก็มีให้เห็นครับ 😂
</p>
<p>
    และด้วยความที่สตรีมเมอร์เพื่อนบ้านของผมหลายๆคนก็ยังใช้ของ Stock ที่เป็น Text เปลือยๆ
    เห็นแล้วก็น่ากำหมัดเล็กน้อย<s>ที่ไม่น้อย</s> ประจบกับผมต้องการหาโปรเจค dev มาหาทำนอกงานประจำที่ไม่ใช่งานวาด
    ก็เลยเกิดเป็นโปรเจคนี้ขึ้นมาครับ
</p>
<p>
    ทีนี้ เรามาดูสเต็ปต่างๆที่ต้องเตรียมการกันก่อน Suffer นะครับ อย่างแรกเลยคือ การตรัสรู้ว่า API Streamlabs
    มันส่งอะไรมาบ้าง สาเหตุที่ผมใช้ของ Streamlabs ก็เพราะผมใช้ Chat Overlay ตัวดั้งเดิมของมันอยู่แล้ว
    เลยเอามาต่อยอดให้สุดครับ แต่มันติดปัญหาที่ว่า
    <strong>
        ผมพลิกแผ่นดิน พลิกมหาสมุทรหา Documentation ของ Streamlabs API ก็หาไม่เจอครับ
    </strong>
    แบบว่า ไม่จริงน่า! ระบบที่คนใช้ก็ตั้งเยอะ แต่ไม่มี Documentation เลยเนี่ยนะ บ้าไปแล้ว 💀
    แล้วผมก็กุมขมับไปวันนึงเต็มๆ จนกระทั่งผมสังเกตบรรทัดนึงในโค้ด Javascript ของ Default Chat Overlay ของ Streamlabs
    ครับว่า
</p>
<p>
    <code>document.addEventListener('onEventReceived', function(obj) {});</code>
</p>
<p>
    ทันใดนั้นครับ ผมก็มีความคิดที่ว่า ถ้าเรา <code>console.log(obj);</code> มาทั้งก้อน มันจะออกมายังไงหว่า
    เท่านั้นแหละครับ ออกมาทั้ง JSON Object แบบ บอกหมดเลยว่ามี Parameters อะไรให้ใช้บ้าง ความรู้สึกแรกเลยคือ โย่ว
    เสร็จชั้นล่ะ
    Hehe Boi 😈
</p>
<p>
    แต่เอาจริงๆทั้งก้อนมันก็เยอะกว่าที่ต้องการแหละครับ ทีนี้เรามาดูกันดีกว่าว่า ตัวที่เราจำเป็นต้องใช้มีอะไรบ้าง ตัวอย่างนี้จะเป็นของแชทสลับข้างนะครับ
    โดยที่ Streamer และ Mod จะอยู่ชิดซ้าย Role อื่นๆจะอยู่ชิดขวาครับ ส่วนแบบอื่นๆอีกสองแบบเดี๋ยวกล่าวอีกทีนะครับ
</p>
<ul>
    <li>
        <code>obj.detail.messageId</code>
    </li>
    <li>
        <code>obj.detail.subscriber</code>
    </li>
    <li>
        <code>obj.detail.tags.vip</code>
    </li>
    <li>
        <code>obj.detail.tags.mod</code>
    </li>
    <li>
        <code>obj.detail.owner</code>
    </li>
    <li>
        <code>obj.detail.tags.color</code>
    </li>
</ul>
<p>
    ซึ่ง Parameters ต่างๆนี้ เราสามารถเอามาใช้ให้กรอบคำพูดแชทแยกตาม Role ของช่องได้ครับ
</p>
<h3>CSS</h3>
<p>
    อย่างเช่นตัวพื้นฐานที่ผมทำมาก็จะมี Streamer พื้นสีแดง, Mod พื้นสีเขียว, VIP พื้นสีชมพู, Sub พื้นสีขาว (ซึ่งต้องทำให้
    Text
    เป็นสีดำแทน โดยแก้สีฟอนต์ที่ <code>.username_box_sub</code> ครับ), ผู้แชทธรรมดา พื้นสีดำ ตาม Class ในไฟล์ CSS
    โดยสัญลักษณ์บอก Class คือจุด <code>.</code> ครับ -
    <code>.wrapper-streamer</code>,
    <code>.wrapper-mod</code>,
    <code>.wrapper-vip</code>,
    <code>.wrapper-sub</code> ผู้แชทธรรมดา จะเป็นแค่ <code>.wrapper</code> ครับ
</p>
<p>
    สำหรับในส่วน <code>.wrapper-role:before</code> หรือ <code>.wrapper-role:after</code>
    นี่ก็จะเป็นส่วนที่เป็นติ่งสามเหลี่ยมหน้าหรือหลังกล่องที่แสดงข้อความครับ ถ้าเปลี่ยนสีพื้น Role
    ไหนก็อย่าลืมเปลี่ยนตรงนี้ตาม Role ด้วยนะครับ
</p>
<p>
    ในส่วนของ Badge ผมได้ใส่ไว้อยู่ทั้งหมด 2 ตำแหน่งในกล่องข้อความหนึ่งกล่อง แล้วใช้วิธีปิด Class และการแสดงผลตาม Role ครับ จะเป็นมุมซ้ายและขวาบนของกล่องนะครับ
</p>
<h3>Javascript</h3>
<p>
    สำหรับในส่วนของ Javascript นะครับ หรือที่ทุกคนพอจะนึกภาพได้ว่าตรงนี้แหละ คือ True Coding/Programming เป็นส่วนของ
    การทำงานใช้ Logic นะครับ ซึ่งในส่วนของการอัพเดตแชทแบบ Real Time ก็คือ ให้ตรวจจับตามอัพเดทแชท หรือบรรทัดที่เขียนว่า
    <code>document.addEventListener('onEventReceived', function(obj) {});</code> นะครับ
    โดยในวงเล็บปีกกา <code>{}</code> ก็จะเป็นชุดคำสั่งว่า เราจะต้องทำอะไรกับข้อมูลบ้างทุกครั้งที่แชทอัพเดตนะครับ
</p>
<p>
    ขั้นแรกคือการแปะ ID ตามกล่องครับ โดยจะมีการแปะที่ตัวกล่อง ชื่อผู้แชท และข้อความของผู้แชท เพื่อจำแนก
    ว่าข้อความนี้ต้องใช้ข้อมูลอะไรในการเปลี่ยนการแสดงผลต่อไป ตามนี้
</p>
<ul>
    <li>
        <code>var messageID = obj.detail.messageId+'-wrapper';</code>
    </li>
    <li>
        <code>var messageFrom = obj.detail.messageId+'-username-box';</code>
    </li>
    <li>
        <code>var messageBody = obj.detail.messageId+'-message-box';</code>
    </li>
</ul>
<p>
    เสร็จแล้วก็จะเป็นการดึง Role ของผู้แชทจาก JSON มาเก็บไว้ให้เช็คต่อง่ายๆครับ
</p>
<ul>
    <li>
        <code>var isSub = obj.detail.subscriber;</code>
    </li>
    <li>
        <code>var isVIP = obj.detail.tags.vip;</code>
    </li>
    <li>
        <code>var isMod = obj.detail.tags.mod;</code>
    </li>
    <li>
        <code>var isStreamer = obj.detail.owner;</code>
    </li>
</ul>
<p>
    จากนั้นก็จะเป็นการเช็ค if-else ตาม Role เลยครับ เรียกจาก if แรกที่เป็นเจ้าของช่อง else if ที่เป็น Mod, VIP, Sub
    และปิดท้ายด้วย else ผู้แชททั่วไปครับ ขอยกตัวอย่างการเช็คว่าเป็น Subscriber ในแชทสลับด้านแล้วกันนะครับ
</p>
<pre><code>else if (isSub) { // Subscriber
    var elementWrapper = document.getElementById(messageID);
    elementWrapper.classList.add("wrapper-sub");
    elementWrapper.classList.remove("wrapper");
</code></pre>
<p>
    อันแรกก็จะเป็นการดึงกล่องที่ถูกต้องจาก ID ที่เราแปะไว้นะครับ โดยการใช้ <code>document.getElementById(messageID);</code>
    เราก็จะได้เลือกกล่องนั้นมาสลับใส่ Class ที่ถูกต้องตาม Role โดยการเพิ่ม <code>elementWrapper.classList.add("wrapper-sub");</code>
    และลบ Class <code>elementWrapper.classList.remove("wrapper");</code> ครับ 
</p>
<pre><code>    if (obj.detail.tags.hasOwnProperty('color')) {
    elementWrapper.style.borderRightColor = obj.detail.tags.color;
    }
    else {
    elementWrapper.style.borderRightColor = 'white';
    }
</code></pre>
<p>
    จากนั้นจะเป็นการดึงข้อมูลสีชื่อผู้แชท <code>obj.detail.tags.color</code> มาใส่ที่ขอบครับ โดยใช้การเช็คว่าถ้ามีสีก็จะใช้ค่าสีครับ <code>if (obj.detail.tags.hasOwnProperty('color'))</code>
    ซึ่งในกรณี Subscriber นี้จะอยู่ทางขวา ก็จะใช้การเปลี่ยนสีขอบที่กล่องตามนี้ครับ <code>elementWrapper.style.borderRightColor = obj.detail.tags.color;</code>
</p>
<p> ถ้าไม่สามารถดึงข้อมูลได้ก็จะใช้สีขาวตาม else นั่นเอง</p>
<pre><code>    document.getElementById(obj.detail.messageId+'-d-none-right-inv').style.display = "none";
    document.getElementById(obj.detail.messageId+'-d-none-right-top').style.display = "none";
    document.getElementById(obj.detail.messageId+'-meta').style.textAlign = "right";
    document.getElementById(obj.detail.messageId+'-message-box').style.textAlign = "right";
</code></pre>
<p>
    ต่อไปจะเป็นการปิดการแสดงผล badge ในข้างที่ผิดนะครับ เนื่องจากเราได้วาง badge ไว้ทั้งสองข้างใน HTML เราจึงต้อง
    ทำการใส่ <code>.style.display: "none";</code> ในด้านที่ผิดครับ เช่น กล่อง Sub ชิดขวา ก็ต้องปิดขวาให้แสดงผลแต่ซ้าย
    โดยใช้ <code>.style.textAlign = "right";</code> ครับ
</p>
<p>
    อีกสิ่งที่ต้องทำคือจัด text-align ให้ชิดซ้ายชิดควาตามด้านครับ อย่างอันนี้ก็เป็น <code>.style.textAlign = "right";</code>
</p>
<pre><code>    var elementFrom = document.getElementById(messageFrom);
    elementFrom.classList.add("username_box_sub");
    elementFrom.classList.remove("username_box");
    var elementBody = document.getElementById(messageBody);
    elementBody.classList.add("message_box_sub");
    elementBody.classList.remove("message_box");
}
</code></pre>
<p>
    ส่วนสุดท้าย เป็นเรื่อง optional ของสีพื้นกล่องสีกว่าที่ต้องปรับให้สีฟอนต์เป็นสีเข้มครับ เหมือนเดิมครับ เพิ่ม-ลบ Class
</p>
<p>
    โดยที่ผลสุดท้ายก็จะออกมาเป็นตามที่เห็นคล้ายๆด้านล่างเลยครับ เพียงแค่ว่ามันเป็นแชทของผมเอง ซึ่งผมเซ็ตไว้ให้เช็คแค่ Streamer, Sub, และผู้แชททั่วไปครับ แล้วก็เป็นชิดขวาทั้งหมด
</p>
<p>
    สำหรับ Release 1.0 นี้ ผมได้จัดเตรียมไว้ให้สามแบบครับ
</p>
<ol>
    <li>
        ชิดซ้าย <code>twitch-chat-left</code>
    </li>
    <li>
        ชิดขวา <code>twitch-chat-right</code>
    </li>
    <li>
        สลับซ้ายขวา (สตรีมเมอร์และ Mod อยู่ซ้าย คนอื่นอยู่ขวา) <code>twitch-chat-alt</code>
    </li>
</ol>
<p>
    โดยที่วิธีใช้ก็คือสามารถ Copy Paste ไปวางช่อง HTML, CSS, Javascript ได้เลยครับ แล้วผมก็แถมด้วยสไตล์ที่ผมใช้เองด้วย <code>lucian-chat</code> ใครอยากเอาไปใช้ก็
    ตามสะดวกเลยครับ อ่านอีกรอบได้ที่ <a href="#">Blogpost</a> นะครับ
</p>
<p>อย่างไรก็ดี ก็ขอให้สตรีมเมอร์ทุกท่านมีความสุขกับการสตรีมนะครับ วันนี้มีเพียงเท่านี้ ใครมีคำถามอยากไปแงะเองก็สอบถามได้ตามช่องทางติดต่อต่างๆนะครับ</p>
<p>Farewell, lads and lassies.</p>