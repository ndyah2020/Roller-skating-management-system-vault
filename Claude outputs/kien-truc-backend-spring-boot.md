# Kiến trúc & lộ trình học Backend Java Spring Boot — Hệ thống quản lý CLB dạy patin

Tài liệu tra cứu · viết 27/09/2026 · dành cho người đã biết khái niệm backend nói chung (REST API, MVC, database, auth) và core Java OOP, nhưng **chưa từng động vào Spring Boot**

> Đây là tài liệu **kiến trúc & giải thích** — bước "học" trước khi bước "làm". Chưa có dòng code nghiệp vụ nào được viết ra ở đợt này (theo đúng lựa chọn của bạn). Khi bạn đã đọc và nắm được bức tranh tổng thể, buổi làm việc kế tiếp với Claude sẽ bắt đầu viết code thật, đi từng module một, theo đúng lộ trình ở mục 11.
>
> Tài liệu này giả định bạn đã đọc `ban-do-module.md` và `ke-hoach-phan-tich-he-thong.md` trong project — mọi ví dụ ở đây đều lấy thẳng từ 10 module, 62 bảng và các quy tắc nghiệp vụ (BR-01..32) bạn đã chốt ở đó, không bịa thêm domain mới.

---

## Mục lục

1. [Bức tranh tổng thể: Spring / Spring Boot là gì](#1)
2. [Phiên bản & công cụ dùng cho project này](#2)
3. [Quy trình setup từng bước](#3)
4. [Kiến trúc phân tầng & cách tổ chức package](#4)
5. [Vòng đời một request — kể bằng chính quy trình điểm danh (P3) của bạn](#5)
6. [Tầng dữ liệu: JPA/Hibernate & các quy ước bạn đã chốt](#6)
7. [Bảo mật: JWT & phân quyền 3 vai trò](#7)
8. [Validate dữ liệu & xử lý lỗi tập trung](#8)
9. [Kiểm thử cơ bản](#9)
10. [Triển khai thật (deploy)](#10)
11. [Lộ trình học & xây dựng đề xuất](#11)
12. [Bảng thuật ngữ nhanh: Spring ↔ khái niệm chung](#12)
13. [Nguồn tham khảo phiên bản](#13)

---

<a id="1"></a>

## 1. Bức tranh tổng thể: Spring / Spring Boot là gì

### 1.1 Spring Framework vs Spring Boot

**Spring Framework** là một framework Java ra đời để giải quyết một vấn đề rất OOP: khi chương trình lớn dần, các class phụ thuộc chằng chịt vào nhau (class A cần `new` ra class B, B cần `new` ra C...), code trở nên cứng và khó test. Spring giải quyết bằng **IoC (Inversion of Control)** — thay vì A tự tay `new B()`, bạn khai báo "A cần một B", và một cái **container** sẽ tự tạo B và "tiêm" (inject) vào A. Đây gọi là **Dependency Injection (DI)**, là ý tưởng lõi của toàn bộ hệ sinh thái Spring.

Vấn đề của Spring Framework thuần là **cấu hình quá nhiều** — trước đây phải khai báo bean bằng file XML dài dằng dặc. **Spring Boot** ra đời để giải quyết đúng việc đó: nó không phải một framework khác, mà là một lớp "đóng gói + tự động cấu hình" nằm trên Spring Framework, với 3 lời hứa:

| Lời hứa | Nghĩa là gì |
|---|---|
| **Auto-configuration** | Spring Boot đoán bạn cần gì dựa vào các thư viện có trong classpath, rồi tự cấu hình sẵn. Bạn thêm `spring-boot-starter-data-jpa` vào project → nó tự tạo sẵn kết nối JPA, chỉ cần bạn khai báo `application.yml` |
| **Starter dependencies** | Thay vì tự tìm và ghép hàng chục thư viện tương thích nhau, bạn chỉ cần khai báo một "starter" (vd `spring-boot-starter-web`), nó kéo theo đúng bộ thư viện đã được kiểm chứng hoạt động tốt với nhau |
| **Embedded server** | Project build ra một file `.jar` **chạy được ngay** bằng `java -jar`, có sẵn web server (Tomcat) nhúng bên trong — không cần cài Tomcat riêng, không cần deploy file `.war` vào server ngoài như thời Java EE cũ |

Nói cách khác: **Spring = bộ khung DI + các module (Web, Data, Security...). Spring Boot = lớp "cắm là chạy" giúp bạn dùng Spring mà gần như không phải cấu hình tay.** Khi người ta nói "học Spring Boot", 95% thời gian thực chất là học cách dùng đúng các annotation của Spring Framework, Spring MVC, Spring Data, Spring Security — Spring Boot chỉ là lớp vỏ giúp khởi động nhanh.

### 1.2 Bean và ApplicationContext — hai từ khoá sẽ gặp suốt

- **Bean**: một object bất kỳ mà bạn giao cho Spring quản lý vòng đời (tạo, giữ, huỷ) thay vì tự `new`. Đánh dấu một class là bean bằng annotation như `@Component`, `@Service`, `@Repository`, `@Controller` (đây là 4 cái tên khác nhau nhưng về bản chất kỹ thuật là **cùng một cơ chế** — chỉ khác ý nghĩa ngữ nghĩa, để code dễ đọc và để Spring áp một số xử lý đặc thù theo tầng, ví dụ `@Repository` tự dịch exception của tầng DB sang exception chuẩn của Spring).
- **ApplicationContext**: cái "container" giữ toàn bộ bean, chịu trách nhiệm tạo bean theo đúng thứ tự phụ thuộc và tiêm vào nhau. Bạn hiếm khi động trực tiếp vào nó — nó chạy ngầm khi ứng dụng khởi động.
- **Dependency Injection trong thực tế**: cách được khuyến nghị hiện nay là **constructor injection** — khai báo dependency là field `final`, nhận qua constructor. Đây chính là kiểu lập trình hướng OOP bạn đã quen (constructor bắt buộc truyền tham số), Spring chỉ tự động hoá việc gọi constructor đó:

```java
@Service
public class AttendanceService {

    private final AttendanceRepository attendanceRepository; // final -> bắt buộc có ở constructor
    private final SessionRepository sessionRepository;

    // Spring tự tìm bean AttendanceRepository và SessionRepository rồi truyền vào đây
    public AttendanceService(AttendanceRepository attendanceRepository,
                              SessionRepository sessionRepository) {
        this.attendanceRepository = attendanceRepository;
        this.sessionRepository = sessionRepository;
    }
}
```

Không cần annotation `@Autowired` trên constructor nếu class chỉ có **một** constructor — Spring tự hiểu. Đây là điểm khác với nhiều tutorial cũ hay dùng `@Autowired` trên field (`@Autowired private Foo foo;`) — cách đó vẫn chạy nhưng bị xem là *anti-pattern* (khó test, che giấu dependency thật sự), tài liệu này sẽ dùng constructor injection xuyên suốt.

### 1.3 Vì sao Spring Boot hợp với hệ thống của bạn

Đối chiếu với `ke-hoach-phan-tich-he-thong.md`:

| Đặc điểm hệ thống của bạn | Vì sao Spring Boot đáp ứng tốt |
|---|---|
| 3 loại client khác nhau (Admin trên máy tính, HLV/Phụ huynh trên điện thoại — NT-5) | Backend chỉ cần expose **REST API** dạng JSON, không quan tâm client là web hay app — Spring Web (Spring MVC) sinh ra cho việc này |
| 62 bảng, quan hệ chặt, nhiều ràng buộc (UNIQUE, FK, `is_active`, đa hình) | Spring Data JPA + PostgreSQL là cặp đôi tiêu chuẩn cho dữ liệu quan hệ phức tạp, có transaction thật (ACID) — rất cần cho các quy tắc như BR-10, BR-15 |
| 3 vai trò, ma trận quyền rõ theo module (mục 2 trong `ke-hoach-phan-tich-he-thong.md`) | Spring Security có model phân quyền theo role/permission rất trưởng thành, tích hợp thẳng vào từng endpoint |
| Hệ thống chạm tiền và tài sản, đòi hỏi audit (`created_by`, `updated_by`, không xoá cứng — BR-30, BR-31) | `@Transactional` đảm bảo một nghiệp vụ (vd chốt ngày) hoặc thành công trọn vẹn hoặc không thay đổi gì, tránh nửa vời |
| Sẽ deploy thật, chạy dài hạn | Spring Boot có sẵn cơ chế production-ready (health check, cấu hình theo môi trường, ecosystem lớn, tài liệu nhiều) |

---

<a id="2"></a>

## 2. Phiên bản & công cụ dùng cho project này

Phần này **đã tra cứu thực tế tại thời điểm viết (27/09/2026)** thay vì dùng số phiên bản cũ trong trí nhớ — nguồn ở mục 13.

| Thành phần | Chọn | Vì sao |
|---|---|---|
| **Java** | **25 (LTS)** | Bản LTS (Long-Term Support) mới nhất, được hỗ trợ tới 09/2030. Java 26, 27 mới hơn nhưng là bản *thường* (non-LTS), hết hỗ trợ sau 6 tháng — không hợp cho hệ thống chạy dài hạn |
| **Spring Boot** | **4.1.x** | Bản ổn định mới nhất dòng đang được hỗ trợ (dòng 3.5 vừa hết hỗ trợ OSS đúng tháng này). Yêu cầu tối thiểu Java 17, khuyến khích Java 21/25 để tận dụng *virtual thread* |
| **Build tool** | **Maven** | Theo lựa chọn của bạn — cấu hình bằng file `pom.xml`, XML tường minh, dễ tra cứu khi mới học |
| **Database** | **PostgreSQL** | Đã được ngầm định qua các quy ước bạn chốt trong `cau-truc-obsidian.md`: kiểu `timestamptz`, `numeric(5,2)`, UUID PK — đây đều là các kiểu/thuật ngữ của PostgreSQL. Dùng đúng Postgres để các quy ước đó ánh xạ tự nhiên |
| **IDE** | IntelliJ IDEA (bản Community, miễn phí) | Hỗ trợ Spring/JPA tốt nhất hiện có: tự nhận annotation, gợi ý tên cột, chạy app một nút bấm. VS Code + extension "Spring Boot Extension Pack" là lựa chọn thay thế ổn |

### Lưu ý quan trọng về việc học với Spring Boot 4

Spring Boot 4 (Spring Framework 7) mới GA từ 11/2025, nghĩa là phần lớn tutorial/video/StackOverflow answer có sẵn trên mạng vẫn viết cho **Spring Boot 3.x**. Tin tốt: khác biệt với người mới học không lớn — API cốt lõi (`@RestController`, `@Service`, `@Entity`, Spring Data JPA, Spring Security 6/7...) gần như không đổi. Vài điểm nếu bạn copy code từ tutorial cũ mà gặp lỗi biên dịch thì khả năng cao là do:

- **Jackson 3.x bắt buộc** (Spring Boot 4 không còn hỗ trợ Jackson 2.x) — hiếm khi bạn tự cấu hình Jackson nên ít gặp
- **JUnit 4 bị loại bỏ hoàn toàn**, chỉ còn JUnit Jupiter (JUnit 5/6) — nếu thấy tutorial dùng `import org.junit.Test` (không có `.jupiter.`) thì đó là code cũ, bỏ qua
- Namespace `jakarta.*` thay vì `javax.*` — điều này thật ra đã đổi từ Spring Boot 3 (2022), nên hầu hết tutorial còn tồn tại trên mạng đã cập nhật rồi
- **`WebSecurityConfigurerAdapter` đã bị xoá từ lâu** (Spring Security 5.7+) — nếu thấy tutorial security dùng class này, đó là tài liệu rất cũ (Spring Boot 2.x trở về trước), bỏ qua hoàn toàn, dùng cách khai báo `SecurityFilterChain` như mục 7 dưới đây

### Danh sách dependency (chọn khi tạo project ở Spring Initializr — mục 3.2)

| Dependency | Dùng để làm gì trong hệ thống này |
|---|---|
| **Spring Web** | Viết REST API (`@RestController`) cho Admin/HLV/Phụ huynh gọi vào |
| **Spring Data JPA** | Map 62 bảng sang Java entity, sinh sẵn các thao tác CRUD |
| **PostgreSQL Driver** | Driver JDBC để Java nói chuyện được với Postgres |
| **Spring Security** | Đăng nhập, phân quyền Admin/HLV/Phụ huynh |
| **Validation** | Kiểm tra dữ liệu đầu vào (`@NotBlank`, `@Min`...) trước khi chạm tới nghiệp vụ |
| **Flyway Migration** | Quản lý version cho schema 62 bảng — xem lý do bắt buộc phải có ở mục 6.2 |
| **Lombok** | Giảm code lặp (getter/setter) cho entity — xem mục 4.3 |
| **Spring Boot DevTools** *(tuỳ chọn)* | Tự restart app khi bạn sửa code lúc dev, đỡ phải tắt/bật tay |

JWT library **không có sẵn trong danh sách Spring Initializr** vì Spring Boot không đi kèm một thư viện JWT mặc định — bạn sẽ thêm thủ công `io.jsonwebtoken:jjwt-api`, `jjwt-impl`, `jjwt-jackson` vào `pom.xml` sau (chi tiết ở mục 7).

---

<a id="3"></a>

## 3. Quy trình setup từng bước

### 3.1 Cài đặt môi trường

1. **JDK 25** — tải bản Eclipse Temurin (bản build OpenJDK miễn phí, ổn định) tại adoptium.net, chọn đúng Java 25 LTS. Sau khi cài, kiểm tra bằng `java -version`.
2. **Maven** — thật ra **không bắt buộc cài riêng**: khi tạo project qua Spring Initializr, nó kèm sẵn file `mvnw`/`mvnw.cmd` (Maven Wrapper) — chạy `./mvnw` (hoặc `mvnw.cmd` trên Windows) thay cho `mvn`, không cần cài Maven thủ công, luôn đúng version Maven mà project khai báo.
3. **IDE**: cài IntelliJ IDEA Community.
4. **PostgreSQL** cho môi trường dev — hai lựa chọn, chọn một:
   - **Docker Compose** (khuyến nghị nếu máy bạn đã có Docker Desktop): chỉ cần 1 file `docker-compose.yml` khai báo Postgres, gõ `docker compose up -d`, không cài gì thêm lên máy, xoá sạch bằng `docker compose down -v` khi cần làm lại từ đầu.
   - **Cài Postgres trực tiếp lên máy**: tải PostgreSQL installer, nhớ mật khẩu user `postgres` lúc cài, dùng pgAdmin (đi kèm) để tạo database mới.

### 3.2 Tạo project bằng Spring Initializr

Vào `start.spring.io`, điền:

| Trường | Giá trị gợi ý |
|---|---|
| Project | Maven |
| Language | Java |
| Spring Boot | bản ổn định mới nhất (4.1.x) |
| Group | `com.skatingclub` (hoặc theo tên bạn muốn) |
| Artifact / Name | `management-system` |
| Package name | `com.skatingclub.management` |
| Packaging | Jar |
| Java | 25 |
| Dependencies | 8 mục ở bảng trên (Web, Data JPA, PostgreSQL, Security, Validation, Flyway, Lombok, DevTools) |

Bấm **Generate**, giải nén, mở bằng IntelliJ (chọn mở qua file `pom.xml`).

### 3.3 Giải phẫu project vừa tạo

```
management-system/
├── pom.xml                                  # "danh sách nguyên liệu" — khai báo mọi dependency
├── mvnw, mvnw.cmd                           # Maven Wrapper — chạy Maven không cần cài
├── src/
│   ├── main/
│   │   ├── java/com/skatingclub/management/
│   │   │   └── ManagementSystemApplication.java   # điểm khởi động (có hàm main)
│   │   └── resources/
│   │       ├── application.yml              # toàn bộ cấu hình (DB, port, profile...)
│   │       └── db/migration/                # bạn sẽ tự tạo — nơi để các file Flyway
│   └── test/
│       └── java/com/skatingclub/management/
│           └── ManagementSystemApplicationTests.java
```

`ManagementSystemApplication.java` trông như sau — đây là **toàn bộ** code cần để có một ứng dụng Spring Boot chạy được:

```java
@SpringBootApplication
public class ManagementSystemApplication {
    public static void main(String[] args) {
        SpringApplication.run(ManagementSystemApplication.class, args);
    }
}
```

`@SpringBootApplication` thực chất là gộp của 3 annotation (`@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`) — nó nói với Spring Boot: "hãy tự động cấu hình mọi thứ, và quét toàn bộ package con của package này để tìm bean (`@Service`, `@Controller`...)". Đây là lý do **vị trí package của class này quan trọng**: nó phải nằm ở gốc, để `@ComponentScan` quét được xuống mọi module con (`ht`, `dh`, `hv`...).

### 3.4 Cấu hình `application.yml` và chạy thử lần đầu

Đổi tên `application.properties` (mặc định) thành `application.yml` (dễ đọc hơn với cấu trúc phân cấp — không bắt buộc, nhưng đa số project Spring Boot hiện nay dùng YAML):

```yaml
spring:
  application:
    name: management-system

  datasource:
    url: jdbc:postgresql://localhost:5432/patin_club
    username: patin_app
    password: ${DB_PASSWORD:changeme}   # đọc từ biến môi trường, mặc định "changeme" lúc dev

  jpa:
    hibernate:
      ddl-auto: validate   # KHÔNG dùng "update"/"create" cho hệ thống thật — xem mục 6.2
    open-in-view: false    # tắt tính năng gây rò rỉ transaction ra ngoài tầng service — nên tắt luôn từ đầu

  flyway:
    enabled: true
    locations: classpath:db/migration

server:
  port: 8080
```

Chạy bằng `./mvnw spring-boot:run` (hoặc bấm nút Run trong IntelliJ trên file `ManagementSystemApplication`). Nhìn log khởi động, bạn sẽ thấy vài dòng đáng chú ý:

- `Tomcat started on port 8080` → đây là **embedded Tomcat** — một web server thật đang chạy *bên trong* tiến trình Java của bạn, không phải service riêng
- `HikariPool ... Start completed` → connection pool tới Postgres đã sẵn sàng (Spring Boot tự dùng HikariCP, connection pool nhanh nhất hiện nay, không cần bạn cấu hình gì thêm)
- Nếu có file migration trong `db/migration`, bạn sẽ thấy Flyway log việc chạy từng file `V1__...`, `V2__...`

Nếu log dừng lại với dòng lỗi kết nối Postgres → kiểm tra lại bước 3.1 (Postgres đã chạy chưa, đúng port 5432, đúng tên database `patin_club` đã tạo chưa).

---

<a id="4"></a>

## 4. Kiến trúc phân tầng & cách tổ chức package

### 4.1 Package theo module (feature) chứ không theo tầng

Có hai trường phái tổ chức code:

- **Package theo tầng** (`controller/`, `service/`, `repository/` ở gốc, mỗi thư mục chứa lẫn lộn class của mọi module) — dễ thấy trong tutorial nhỏ, nhưng với hệ thống 10 module/62 bảng như của bạn, thư mục `service/` sẽ có tới hơn 30-40 file lẫn lộn, rất khó định vị.
- **Package theo module/tính năng** (mỗi module một package riêng, trong đó mới chia tầng) — khuyến nghị cho project này vì nó **khớp thẳng với 10 module bạn đã phân tích sẵn** (HT, DH, NS, HV, TC, LG, TS, BH, MK, BC). Một module = một package = một đơn vị bạn có thể học/build/test độc lập.

Cây thư mục đề xuất (minh hoạ bằng module **HT — Nền tảng**, module đầu tiên bạn sẽ build):

```
com.skatingclub.management/
├── ManagementSystemApplication.java
│
├── common/                          # dùng chung cho mọi module
│   ├── entity/BaseEntity.java       # id, created_at, updated_at, created_by, updated_by, is_active
│   ├── exception/                   # exception dùng chung + GlobalExceptionHandler
│   ├── config/                      # CORS, OpenAPI/Swagger, Jackson config...
│   └── security/                    # JwtService, JwtAuthFilter, SecurityConfig
│
├── ht/                               # module Nền tảng & phân quyền
│   ├── entity/
│   │   ├── User.java
│   │   ├── Role.java
│   │   ├── Permission.java
│   │   ├── Venue.java
│   │   ├── VenueTimeSlot.java
│   │   ├── Category.java
│   │   └── AuditLog.java
│   ├── repository/
│   │   ├── UserRepository.java
│   │   ├── VenueRepository.java
│   │   └── ...
│   ├── dto/
│   │   ├── request/CreateVenueRequest.java
│   │   └── response/VenueResponse.java
│   ├── service/
│   │   ├── AuthService.java
│   │   └── VenueService.java
│   └── controller/
│       ├── AuthController.java
│       └── VenueController.java
│
├── hv/                                # module Học viên & phụ huynh (giai đoạn 1)
│   └── ... (cấu trúc con giống hệt ht/)
│
├── dh/                                # module Dạy học (giai đoạn 1)
│   └── ...
│
├── tc/                                # module Tài chính & học phí (giai đoạn 1)
│   └── ...
│
└── ... (ns, lg, ts, bh, mk, bc — build ở giai đoạn sau, theo đúng mục 6 trong ke-hoach-phan-tich-he-thong.md)
```

Mỗi module con lặp lại đúng 5 thư mục: `entity → repository → dto → service → controller`. Học xong pattern này ở module `ht`, bạn chỉ việc lặp lại cho `hv`, `dh`... — đây chính là lý do lựa chọn "làm trọn vẹn 1 module" của bạn hợp lý về mặt sư phạm.

### 4.2 Vai trò từng tầng (đọc theo đúng thứ tự một request đi qua)

| Tầng | Annotation chính | Vai trò | Ví dụ đặt tên |
|---|---|---|---|
| **Controller** | `@RestController` | Nhận HTTP request, đọc input, gọi Service, trả HTTP response. **Không chứa logic nghiệp vụ** | `VenueController`, `AttendanceController` |
| **DTO** (Data Transfer Object) | *(không cần annotation Spring)* | Định hình dữ liệu ra/vào API — **không phải là Entity** | `CreateVenueRequest`, `VenueResponse` |
| **Service** | `@Service` | Nơi chứa **toàn bộ logic nghiệp vụ** — đúng là nơi các BR-01..32 của bạn sẽ được viết thành code | `AttendanceService.markAttendance(...)` |
| **Repository** | *(interface, kế thừa `JpaRepository`)* | Giao tiếp với database — Spring Data JPA tự sinh code CRUD | `AttendanceRepository` |
| **Entity** | `@Entity` | Một class Java ánh xạ 1-1 với một bảng trong Postgres | `Attendance`, `Session`, `Student` |

**Vì sao tách DTO khỏi Entity** — đây là câu hỏi mọi người mới học Spring đều thắc mắc ("sao không trả thẳng Entity ra API cho nhanh?"). Ba lý do cụ thể với hệ thống của bạn:

1. **Bảo mật**: `User` entity có `password_hash` — nếu trả thẳng Entity, bạn phải nhớ đánh dấu ẩn field này ở mọi endpoint, rất dễ quên một chỗ và lộ hash mật khẩu ra API.
2. **Tách input khỏi output**: `CreateVenueRequest` (lúc tạo mới, chưa có `id`) khác hình dạng với `VenueResponse` (đã có `id`, `createdAt`). Entity chỉ có một hình dạng, không tách được vậy.
3. **Tránh lỗi lazy-loading khi serialize**: Entity có quan hệ `@ManyToOne`/`@OneToMany` (vd `Session` có thể có nhiều `Attendance`) — nếu Jackson serialize thẳng Entity, nó có thể vô tình kích hoạt load thêm cả bảng liên quan ngoài ý muốn (và lỗi nếu ngoài transaction, càng dễ gặp vì đã tắt `open-in-view`). DTO chỉ chứa đúng field bạn chủ động chọn.

### 4.3 DTO nên viết bằng `record`, Entity thì không

Java `record` (có từ Java 16) là cách khai báo một class bất biến (immutable) chỉ trong 1 dòng — tự có constructor, getter, `equals`/`hashCode`/`toString`:

```java
public record VenueResponse(UUID id, String name, String address, VenueStatus status) {}
```

Dùng `record` cho **mọi DTO** — vì DTO đúng nghĩa là dữ liệu bất biến truyền qua lại, không có logic. **Nhưng không dùng `record` cho `@Entity`** — JPA/Hibernate bắt buộc entity phải có constructor không tham số và field có thể sửa được (để Hibernate tự gán giá trị khi load từ DB, và để tạo proxy phục vụ lazy-loading), điều mà `record` (immutable, không cho subclass) không đáp ứng được. Với Entity, dùng **Lombok** để giảm code lặp:

```java
@Entity
@Table(name = "venue")
@Getter
@Setter
@NoArgsConstructor
public class Venue extends BaseEntity {

    @Column(nullable = false)
    private String name;

    private String address;

    @Enumerated(EnumType.STRING)   // lưu enum dưới dạng text, đúng quy ước bạn đã chốt
    private VenueStatus status;
}
```

`@Getter`/`@Setter`/`@NoArgsConstructor` là annotation của Lombok — chúng chỉ **sinh code lúc biên dịch** (bạn không thấy code đó trong file `.java`, nhưng file `.class` có đủ getter/setter thật). Đây là lý do bạn cần cài plugin Lombok cho IDE, nếu không IDE sẽ báo đỏ dù code vẫn biên dịch được.

### 4.4 `BaseEntity` — nơi đặt 6 cột chung của mọi bảng

`ban-do-module.md` đã liệt kê rõ 6 cột chung: `id`, `created_at`, `updated_at`, `created_by`, `updated_by`, `is_active`. Dùng JPA `@MappedSuperclass` để không phải lặp lại 6 field này ở cả 62 entity:

```java
@MappedSuperclass
@Getter
@Setter
public abstract class BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.UUID)   // Postgres + Hibernate tự sinh UUID
    private UUID id;

    @Column(name = "created_at", updatable = false)
    private Instant createdAt;

    @Column(name = "updated_at")
    private Instant updatedAt;

    private UUID createdBy;
    private UUID updatedBy;

    @Column(name = "is_active", nullable = false)
    private boolean isActive = true;

    @PrePersist
    void onCreate() {
        createdAt = updatedAt = Instant.now();
    }

    @PreUpdate
    void onUpdate() {
        updatedAt = Instant.now();
    }
}
```

`@PrePersist`/`@PreUpdate` là "hook" của JPA — chạy tự động ngay trước khi Hibernate insert/update, đúng chỗ để tự động điền `created_at`/`updated_at` mà không cần nhớ gán tay ở từng Service. (`created_by`/`updated_by` thường cần lấy từ người đang đăng nhập — sẽ gán ở tầng Service khi đã có JWT, xem mục 7.)

### 4.5 Cột đa hình (`item_type` / `item_id`) — vì sao KHÔNG map bằng JPA relationship

`cau-truc-obsidian.md` liệt kê nhiều bảng dùng cặp cột đa hình không có FK cứng: `custody_log`, `invoice_line`, `expense`, `payroll_line`, `referral`, `notification_log`, `alert` (vd `invoice_line.item_type` = `package`/`product`/`rental`/`other`, `item_id` trỏ tới bảng tương ứng theo `item_type`).

JPA/Hibernate *có* một cơ chế cho việc này (`@Any` của Hibernate), nhưng nó phức tạp, ít tài liệu, dễ dùng sai — **không đáng dùng cho người mới học**, kể cả nhiều team senior cũng tránh. Cách thực dụng hơn nhiều: khai báo `itemType` (enum) và `itemId` (UUID) là **field thường, không có annotation quan hệ nào cả**, rồi tự resolve bằng logic if/switch ở tầng Service khi thật sự cần lấy dữ liệu bảng liên quan:

```java
public BigDecimal resolveItemUnitPrice(InvoiceLine line) {
    return switch (line.getItemType()) {
        case PACKAGE -> packageRepository.findById(line.getItemId())
                .orElseThrow().getPrice();
        case PRODUCT -> productRepository.findById(line.getItemId())
                .orElseThrow().getSellPrice();
        case RENTAL, OTHER -> line.getUnitPrice(); // giá đã ghi thẳng lúc tạo dòng
    };
}
```

Đơn giản, tường minh, dễ debug — đúng tinh thần NT-1 (ít thao tác nhất) áp dụng cả cho việc *đọc* code sau này, không chỉ cho người dùng cuối.

---

<a id="5"></a>

## 5. Vòng đời một request — kể bằng chính quy trình điểm danh (P3) của bạn

Phần lý thuyết ở trên sẽ dễ nhớ hơn nhiều khi thấy nó ráp lại với nhau trong một luồng thật. Lấy đúng quy trình **P3 — Điểm danh tại sân** và 4 lớp chống tích trùng bạn đã thiết kế trong `ke-hoach-phan-tich-he-thong.md`, đi theo bước `HLV bấm tích điểm danh cho một học viên`:

```
HLV bấm nút trên điện thoại
        │
        ▼
POST /api/sessions/{sessionId}/attendance   { studentId }
        │  Header: Authorization: Bearer <JWT>
        ▼
① JwtAuthFilter (mục 7)
   Xác thực token, đọc ra user_id + role, gán vào SecurityContext
        │
        ▼
② AttendanceController
   @PreAuthorize đã cho phép role HLV (theo ma trận quyền module DH)
   @Valid kiểm tra DTO đầu vào hợp lệ (mục 8)
        │
        ▼
③ AttendanceService.markAttendance(sessionId, studentId, currentUserId)
   - BR-07: kiểm tra HLV có thoả điều kiện thấy được buổi này không
   - BR-09: không giới hạn HLV nào tích cho học viên nào trong danh sách
   - Tìm attendance đã tồn tại chưa (findBySessionIdAndStudentId)
     → CÓ rồi: BR-11 "người ghi trước thắng", trả về thông tin người đã tích, KHÔNG lỗi
     → CHƯA có: tạo mới, marked_by = currentUserId (BR-08), lưu qua Repository
        │
        ▼
④ AttendanceRepository.save(...)
   Hibernate sinh câu SQL INSERT, chạy trong 1 transaction (@Transactional ở Service)
        │
        ▼
⑤ PostgreSQL
   Ràng buộc UNIQUE (session_id, student_id) — BR-10, lớp chống trùng CHẮC CHẮN nhất
   Nếu 2 request cùng lúc lọt qua bước ③ (race condition) → 1 trong 2 sẽ bị Postgres
   từ chối ở đây → Service bắt exception, coi như "đã có người tích", trả kết quả người thắng
        │
        ▼
⑥ Trả AttendanceResponse (JSON) về Controller → về điện thoại HLV
```

Vài điểm đáng để tâm khi nhìn lại toàn bộ chuỗi này:

- **4 lớp chống trùng của bạn nằm ở 3 tầng khác nhau của Spring Boot**: lớp 1 (ràng buộc CSDL) nằm ở Postgres/migration, lớp 2 (upsert) nằm ở Service, lớp 3 (đồng bộ danh sách) là logic phía frontend polling API định kỳ (ngoài phạm vi backend thuần), lớp 4 (mã lần gửi / idempotency key) sẽ là một field trong DTO request + kiểm tra ở Service. Điều này cho thấy: **kiến trúc phân tầng của Spring Boot không tự động cho bạn sự an toàn — bạn vẫn phải chủ động thiết kế từng lớp phòng thủ**, Spring chỉ cho bạn chỗ (annotation, transaction) để đặt đúng lớp đó vào đúng vị trí.
- Đây cũng là lý do vì sao Service **không được** chỉ gọi `if (existing == null) save()` rồi thôi — luôn phải có khối `try/catch` bắt lỗi vi phạm UNIQUE constraint ở tầng save, vì khoảng thời gian giữa "check" và "save" chính là khe hở race condition mà bạn đã lường trước trong tài liệu phân tích.

---

<a id="6"></a>

## 6. Tầng dữ liệu: JPA/Hibernate & các quy ước bạn đã chốt

### 6.1 Ánh xạ quy ước của bạn sang JPA/Hibernate cụ thể

`cau-truc-obsidian.md` đã chốt sẵn quy ước kiểu dữ liệu — bảng dưới đây là bản dịch trực tiếp sang Java/JPA:

| Quy ước của bạn | Kiểu cột Postgres | Kiểu field Java | Ghi chú JPA |
|---|---|---|---|
| UUID PK | `uuid` | `UUID` | `@GeneratedValue(strategy = GenerationType.UUID)` — Hibernate 6+/Jakarta Persistence 3.2 hỗ trợ thẳng, không cần thư viện ngoài |
| Tiền VND | `bigint` | `Long` | VND không có phần thập phân thực dùng → lưu số nguyên tuyệt đối, tránh sai số dấu phẩy động |
| Số buổi (cho phép 0.5) | `numeric(5,2)` | `BigDecimal` | **Không dùng `double`/`float`** cho bất kỳ giá trị tiền hoặc số buổi nào — sai số dấu phẩy động có thể làm lệch `student_package.session_remaining` |
| Enum lưu text | `varchar` | Java `enum` | `@Enumerated(EnumType.STRING)` — **bắt buộc chỉ định `STRING`**, mặc định của JPA là `ORDINAL` (lưu số thứ tự 0,1,2...) cực kỳ nguy hiểm vì chỉ cần đổi thứ tự khai báo enum trong code là dữ liệu cũ đọc sai nghĩa |
| `timestamptz` | `timestamptz` | `Instant` | Dùng `Instant` (thời điểm tuyệt đối, không gắn múi giờ) chứ không dùng `LocalDateTime` (không có múi giờ) — khớp đúng bản chất "timestamp with time zone" của Postgres, tránh lỗi lệch giờ khi CLB có thể mở thêm sân ở múi giờ khác sau này |
| Cột đa hình (`item_type`/`item_id`) | `varchar` + `uuid`, không FK cứng | enum + `UUID` field thường | Xem mục 4.5 — không map quan hệ JPA |

### 6.2 Vì sao bắt buộc dùng Flyway, không để Hibernate tự tạo bảng

Hibernate có tính năng `spring.jpa.hibernate.ddl-auto=update` — tự động tạo/sửa bảng theo Entity mỗi lần chạy app. **Tiện cho demo, nhưng tuyệt đối không dùng cho hệ thống thật**, vì:

- Không có lịch sử — bạn không biết chính xác schema production đang ở "phiên bản" nào, không rollback được.
- Hibernate tự suy luận kiểu cột từ Entity, và đoán *sai* khá thường xuyên với các quy ước riêng bạn đã chốt (vd nó không tự biết bạn muốn `numeric(5,2)` chứ không phải `numeric(19,2)` mặc định).
- Với dữ liệu **tiền và tài sản** (BR-30) — một lần Hibernate "tự sửa" nhầm cột là rủi ro thật.

**Flyway** giải quyết bằng cách bắt bạn tự viết SQL thuần cho từng thay đổi, đánh số thứ tự, chạy đúng 1 lần và ghi lại lịch sử vào bảng `flyway_schema_history`:

```
src/main/resources/db/migration/
├── V1__create_ht_tables.sql       # user, role, permission, venue...
├── V2__create_hv_tables.sql       # student, guardian...
├── V3__create_dh_tables.sql       # course, session, attendance...
└── V4__create_tc_tables.sql       # package, invoice, payment...
```

Ví dụ `V1__create_ht_tables.sql` (rút gọn, chỉ bảng `venue`):

```sql
CREATE TABLE venue (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name        VARCHAR(255) NOT NULL,
    address     VARCHAR(255),
    status      VARCHAR(20)  NOT NULL DEFAULT 'ACTIVE',
    created_at  TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at  TIMESTAMPTZ  NOT NULL DEFAULT now(),
    created_by  UUID,
    updated_by  UUID,
    is_active   BOOLEAN      NOT NULL DEFAULT true
);
```

Quy tắc bất di bất dịch: **file migration đã chạy rồi thì không sửa lại nữa** — Flyway lưu checksum, sửa file cũ sẽ làm app từ chối khởi động (đúng ý, để tránh production và máy bạn lệch schema mà không hay biết). Muốn đổi gì, luôn tạo file `V{n+1}__...sql` mới. Vì vậy `application.yml` đặt `ddl-auto: validate` (không phải `update`) — nghĩa là Hibernate **chỉ được phép kiểm tra** Entity có khớp với schema Flyway đã tạo hay không, khớp thì chạy, lệch thì báo lỗi ngay lúc khởi động thay vì âm thầm sai lúc runtime.

Trình tự làm việc hằng ngày: sửa/thêm Entity → viết file migration SQL tương ứng tay → chạy app → Flyway tự áp migration → Hibernate validate khớp. Cách này chậm hơn `ddl-auto=update` vài giây mỗi lần, đổi lại là an toàn tuyệt đối cho dữ liệu thật — hoàn toàn xứng đáng theo đúng NT-2, NT-3 bạn đã đặt ra.

---

<a id="7"></a>

## 7. Bảo mật: JWT & phân quyền 3 vai trò

### 7.1 Vì sao chọn JWT (stateless) thay vì session truyền thống

`ke-hoach-phan-tich-he-thong.md` (NT-5 và phần rủi ro) đã tự nêu đúng lý do: **HLV dùng điện thoại tại sân, mạng "chập chờn"**. Session truyền thống (server lưu trạng thái đăng nhập trong bộ nhớ/DB, client giữ cookie) buộc mọi request phải tới đúng server đang giữ session đó, và phức tạp hoá khi scale ra nhiều server sau này. **JWT (JSON Web Token)** thì server không lưu gì cả — mọi thông tin cần thiết (user id, role) được **ký chữ ký số** và nhét thẳng vào token, client tự giữ token đó và gửi kèm mỗi request. Server chỉ cần verify chữ ký, không cần tra cứu gì thêm → nhẹ, chịu tải tốt, không phụ thuộc "phải luôn tới đúng server", rất hợp use-case mobile ở nơi mạng yếu.

### 7.2 Luồng xác thực

```
POST /api/auth/login  { username, password }
        │
        ▼
AuthService: so username → tìm User → so password (BCrypt) → đúng thì:
        │  ký JWT chứa: sub=user_id, role=ADMIN/COACH/GUARDIAN, exp=hạn dùng
        ▼
Trả về access token (và tuỳ chọn refresh token, thời hạn dài hơn, để xin cấp access token
mới mà không bắt HLV đăng nhập lại giữa buổi dạy)
        │
        ▼
Client lưu token, mọi request sau gửi kèm header:
    Authorization: Bearer eyJhbGciOi...
        │
        ▼
JwtAuthFilter (chạy trước mọi Controller) verify chữ ký + hạn dùng
    → hợp lệ: gán Authentication vào SecurityContext, request đi tiếp
    → không hợp lệ/hết hạn: trả 401, dừng ở đây, không tới Controller
```

Mật khẩu **không bao giờ** lưu dạng thường — dùng `BCryptPasswordEncoder` (băm một chiều, có "muối" ngẫu nhiên tự động, chuẩn ngành hiện tại):

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

### 7.3 Khai báo bảo mật kiểu hiện đại (KHÔNG dùng `WebSecurityConfigurerAdapter` đã bị xoá)

```java
@Configuration
@EnableMethodSecurity   // bật @PreAuthorize ở từng method Controller
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http, JwtAuthFilter jwtAuthFilter) throws Exception {
        http
            .csrf(csrf -> csrf.disable())   // API JSON thuần, không dùng cookie -> không cần CSRF token
            .sessionManagement(sm -> sm.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()   // đăng nhập thì chưa cần token
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }
}
```

`JwtAuthFilter` là một `OncePerRequestFilter` tự viết (chạy đúng 1 lần mỗi request) — đọc header, verify token, gán `Authentication`:

```java
@Component
public class JwtAuthFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    public JwtAuthFilter(JwtService jwtService) { this.jwtService = jwtService; }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,
                                     FilterChain chain) throws ServletException, IOException {
        String header = request.getHeader("Authorization");
        if (header != null && header.startsWith("Bearer ")) {
            String token = header.substring(7);
            if (jwtService.isValid(token)) {
                SecurityContextHolder.getContext().setAuthentication(jwtService.toAuthentication(token));
            }
        }
        chain.doFilter(request, response);   // luôn cho đi tiếp — endpoint tự quyết có cần auth hay không
    }
}
```

> **Ghi chú cho việc tự tìm hiểu thêm**: đây là cách viết filter JWT thủ công — trực quan, thấy rõ từng bước, tốt để *học*. Khi bạn quen tay hơn, Spring Security cũng có module `spring-boot-starter-oauth2-resource-server` giúp verify JWT với ít code tay hơn (thường dùng khi có một Identity Provider riêng cấp token). Với hệ thống tự cấp token cho chính mình như CLB của bạn, viết filter thủ công như trên là đủ và dễ hiểu hơn cho giai đoạn học.

### 7.4 Map đúng ma trận quyền theo module bạn đã có

Ma trận quyền trong `ke-hoach-phan-tich-he-thong.md` (mục 2) dịch thẳng sang `@PreAuthorize` ở từng Controller method:

| Module | Admin | HLV | Phụ huynh | Ví dụ `@PreAuthorize` |
|---|---|---|---|---|
| DH — điểm danh buổi mình có mặt | Toàn quyền | ✅ (giới hạn buổi mình được phân) | ❌ | `@PreAuthorize("hasAnyRole('ADMIN','COACH')")` + kiểm tra thêm điều kiện "buổi mình được phân" ngay trong Service (vì đây là điều kiện theo dữ liệu, không chỉ theo role) |
| TC — xem hoá đơn của mình | Toàn quyền | ❌ | ✅ (chỉ của con mình) | `@PreAuthorize("hasRole('ADMIN') or (hasRole('GUARDIAN') and #studentId == authentication.principal.studentId)")` |
| LG — xem phiếu lương | Toàn quyền | ✅ (chỉ của mình) | ❌ | tương tự — role đủ để vào endpoint, Service lọc theo `staff_id` của chính người gọi |

*(`ADMIN`/`COACH`/`GUARDIAN` ở đây chỉ là tên mã minh hoạ cho 3 vai trò Admin/HLV/Phụ huynh — bạn đặt tên `code` thật trong bảng `role` tuỳ ý, chỉ cần khớp giữa dữ liệu và chuỗi truyền vào `hasRole(...)`.)*

Nhận ra pattern: **`@PreAuthorize` trả lời câu "role này có được gọi endpoint này không"**, còn **"chỉ được xem dữ liệu của chính mình" là điều kiện theo dữ liệu, luôn phải kiểm tra thêm ở tầng Service** — Spring Security không tự suy ra được "con của phụ huynh này là ai", đó vẫn là logic nghiệp vụ bạn viết tay, dựa vào `user_id` lấy được từ JWT (`SecurityContextHolder.getContext().getAuthentication()`).

---

<a id="8"></a>

## 8. Validate dữ liệu & xử lý lỗi tập trung

### 8.1 Validate ngay ở DTO, trước khi chạm nghiệp vụ

```java
public record CreateVenueRequest(
    @NotBlank(message = "Tên sân không được để trống")
    String name,

    String address
) {}
```

```java
@PostMapping
public ResponseEntity<VenueResponse> create(@Valid @RequestBody CreateVenueRequest request) {
    return ResponseEntity.status(HttpStatus.CREATED).body(venueService.create(request));
}
```

`@Valid` nói Spring MVC: trước khi vào thân method, hãy kiểm tra mọi ràng buộc (`@NotBlank`, `@Size`, `@Min`...) trên DTO. Sai bất kỳ ràng buộc nào → ném `MethodArgumentNotValidException`, method `create()` **không hề được gọi tới** — validate chặn ngay ở cửa, đúng tinh thần NT-2 (mặc định an toàn).

### 8.2 Bắt lỗi tập trung một chỗ bằng `@RestControllerAdvice`

Không viết `try/catch` lặp lại ở từng Controller — gom về một class xử lý lỗi toàn cục:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ApiError> handleValidation(MethodArgumentNotValidException ex) {
        List<String> errors = ex.getBindingResult().getFieldErrors().stream()
                .map(f -> f.getField() + ": " + f.getDefaultMessage())
                .toList();
        return ResponseEntity.badRequest().body(new ApiError("VALIDATION_ERROR", errors));
    }

    @ExceptionHandler(DuplicateAttendanceException.class)   // exception nghiệp vụ tự định nghĩa — minh hoạ BR-10/BR-11
    public ResponseEntity<ApiError> handleDuplicateAttendance(DuplicateAttendanceException ex) {
        return ResponseEntity.status(HttpStatus.CONFLICT)
                .body(new ApiError("DUPLICATE_ATTENDANCE", List.of(ex.getMessage())));
    }

    @ExceptionHandler(Exception.class)   // lưới an toàn cuối cùng — không để lộ stack trace ra ngoài
    public ResponseEntity<ApiError> handleUnknown(Exception ex) {
        return ResponseEntity.internalServerError()
                .body(new ApiError("INTERNAL_ERROR", List.of("Đã có lỗi xảy ra")));
    }
}

public record ApiError(String code, List<String> messages) {}
```

Mọi lỗi từ mọi module đều trả về đúng một hình dạng JSON (`{code, messages}`) — frontend (web admin lẫn app mobile) chỉ cần viết đúng một đoạn xử lý lỗi chung, không phải đoán hình dạng lỗi khác nhau ở mỗi API.

### 8.3 Exception nghiệp vụ tự định nghĩa — nơi các BR "cấm làm gì" trở thành code

```java
public class DuplicateAttendanceException extends RuntimeException {
    public DuplicateAttendanceException(UUID sessionId, UUID studentId) {
        super("Học viên " + studentId + " đã được điểm danh cho buổi " + sessionId);
    }
}
```

Ném exception này trong `Service` khi tầng dưới (Postgres, qua ràng buộc `UNIQUE (session_id, student_id)` của BR-10) từ chối một lần ghi trùng mà lớp kiểm tra trước đó lỡ bỏ sót — `Controller` không cần biết gì về exception này, `GlobalExceptionHandler` tự bắt và trả đúng mã lỗi HTTP (409 Conflict) kèm thông điệp rõ ràng. Nói rộng ra: **một exception nghiệp vụ tự định nghĩa là cách một BR "không được phép xảy ra" được chuyển thành một loại lỗi rõ ràng, có mã, có thông điệp** — thay vì một `RuntimeException` chung chung không ai biết vì sao.

---

<a id="9"></a>

## 9. Kiểm thử cơ bản

Hệ thống của bạn có nhiều quy tắc **rất dễ viết sai mà khó nhận ra bằng mắt** (BR-10 unique, BR-11 upsert, BR-15 mặc định không trừ) — đúng loại logic nên có test tự động thay vì chỉ tự tay bấm thử trên Postman.

### 9.1 Unit test tầng Service (nhanh, không cần DB thật)

```java
@ExtendWith(MockitoExtension.class)
class AttendanceServiceTest {

    @Mock AttendanceRepository attendanceRepository;
    @InjectMocks AttendanceService attendanceService;

    @Test
    void markAttendance_whenAlreadyMarked_shouldReturnWinnerWithoutError() {   // đúng BR-11
        UUID sessionId = UUID.randomUUID(), studentId = UUID.randomUUID();
        Attendance existing = new Attendance();
        existing.setMarkedBy(UUID.randomUUID());
        when(attendanceRepository.findBySessionIdAndStudentId(sessionId, studentId))
                .thenReturn(Optional.of(existing));

        AttendanceResponse result = attendanceService.markAttendance(sessionId, studentId, UUID.randomUUID());

        assertThat(result.markedBy()).isEqualTo(existing.getMarkedBy());   // vẫn là người tích trước
        verify(attendanceRepository, never()).save(any());                 // không tạo dòng mới
    }
}
```

### 9.2 Integration test (có DB thật, dùng Testcontainers)

Unit test ở trên mock hết Repository nên chạy nhanh, nhưng **không thật sự kiểm tra được ràng buộc `UNIQUE (session_id, student_id)`** — ràng buộc đó nằm ở Postgres, không nằm trong code Java. Muốn test đúng race condition thật, cần một Postgres thật chạy trong lúc test — **Testcontainers** tự bật một container Postgres riêng chỉ để chạy test, huỷ ngay sau khi xong, không đụng tới DB dev của bạn:

```java
@SpringBootTest
@Testcontainers
class AttendanceIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17");

    @DynamicPropertySource
    static void configureDb(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    // test thật: gọi markAttendance 2 lần liên tiếp cho cùng session+student,
    // khẳng định chỉ có đúng 1 dòng attendance được tạo trong DB
}
```

Phần này có thể để **sau** khi đã quen tay với unit test — nêu ở đây để bạn biết công cụ này tồn tại và đúng lúc nào cần tới nó (khi muốn test thật một ràng buộc nằm ở tầng DB).

---

<a id="10"></a>

## 10. Triển khai thật (deploy)

### 10.1 Đóng gói

```bash
./mvnw clean package -DskipTests
java -jar target/management-system-0.0.1-SNAPSHOT.jar
```

Kết quả là **một file `.jar` duy nhất, tự chạy được**, đã gồm cả Tomcat nhúng bên trong — không cần server ngoài.

### 10.2 Docker hoá (khuyến nghị để deploy nhất quán)

`Dockerfile` (multi-stage — build trong container riêng, container chạy thật chỉ chứa file `.jar` đã build xong, gọn và an toàn hơn):

```dockerfile
FROM maven:3.9-eclipse-temurin-25 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:25-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

*(Kiểm tra lại tag `eclipse-temurin:25-jre` còn đúng trên Docker Hub tại thời điểm bạn build — nếu image của Java 25 chưa có/chưa ổn định, dùng tạm `eclipse-temurin:21-jre`, một bản LTS khác vẫn còn được Spring Boot 4.1 hỗ trợ.)*

`docker-compose.yml` để chạy cả app lẫn Postgres cùng lúc, kể cả trên VPS thật:

```yaml
services:
  db:
    image: postgres:17
    environment:
      POSTGRES_DB: patin_club
      POSTGRES_USER: patin_app
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - db_data:/var/lib/postgresql/data

  app:
    build: .
    depends_on: [db]
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/patin_club
      DB_PASSWORD: ${DB_PASSWORD}
    ports:
      - "8080:8080"

volumes:
  db_data:
```

### 10.3 Những việc bắt buộc trước khi để người thật dùng

- **Không bao giờ hardcode mật khẩu/secret trong code hay `application.yml`** — luôn qua biến môi trường (`${DB_PASSWORD}` như trên), file `.env` không commit lên Git.
- **Spring Profile** tách cấu hình dev/prod: `application-dev.yml` (Postgres local, log chi tiết) và `application-prod.yml` (Postgres thật, log gọn hơn), chọn bằng biến môi trường `SPRING_PROFILES_ACTIVE=prod`.
- **HTTPS**: đặt Nginx hoặc Caddy phía trước làm reverse proxy, xin chứng chỉ miễn phí qua Let's Encrypt — HLV/phụ huynh nhập thông tin cá nhân và thanh toán qua app, bắt buộc phải có HTTPS, không có ngoại lệ.
- **Backup Postgres định kỳ** — `pg_dump` theo lịch (cron) trước khi tính tới chuyện gì phức tạp hơn; với dữ liệu tiền và tài sản (BR-30, BR-31), mất dữ liệu là rủi ro nặng nhất của cả hệ thống.
- **Spring Boot Actuator** (thêm dependency `spring-boot-starter-actuator`) cho endpoint `/actuator/health` — dùng để hạ tầng (hoặc chính bạn) kiểm tra app còn sống hay không, gần như bắt buộc có trước khi để chạy thật không ai canh.

---

<a id="11"></a>

## 11. Lộ trình học & xây dựng đề xuất

Bám sát đúng 4 giai đoạn đã có sẵn trong `ke-hoach-phan-tich-he-thong.md` (mục 6) — không tạo thêm phân kỳ mới, chỉ chia nhỏ hơn để học từng bước:

| Bước | Việc làm | Mục tiêu học được gì |
|---|---|---|
| **0** | Làm theo mục 3 tài liệu này: cài môi trường, tạo project, chạy "Hello World" | Thấy Spring Boot khởi động, hiểu embedded server |
| **1** | 1 entity đơn giản không quan hệ: `Venue` — Entity → Migration → Repository → Service → Controller CRUD đầy đủ | Nắm trọn 5 tầng cho trường hợp dễ nhất |
| **2** | `User`/`Role` + đăng nhập trả JWT + `SecurityConfig` + `JwtAuthFilter` | Hiểu bảo mật — **bắt buộc xong bước này thì các module sau mới bảo vệ được endpoint** |
| **3** | Hoàn thiện module **HT** còn lại (`Permission`, `VenueTimeSlot`, `Category`, `AuditLog`) + `@PreAuthorize` theo ma trận quyền | Áp dụng phân quyền thật trên nhiều endpoint |
| **4** | Module **HV** (học viên, phụ huynh, ghi danh) | Thực hành quan hệ nhiều-nhiều qua bảng trung gian (`student_guardian`: một học viên có nhiều người giám hộ, một phụ huynh có nhiều con) |
| **5** | Module **DH** — phần khó nhất: `session`, `attendance`, `credit_transaction`, đúng luồng P2/P3/P4 | Thực hành `@Transactional`, xử lý race condition (mục 5), toàn bộ BR-01..20 |
| **6** | Module **TC** — gói học, hoá đơn, thanh toán | Hoàn tất Giai đoạn 1 — hệ thống đã "thay được sổ giấy" như mục tiêu bạn đặt ra |
| **7+** | Giai đoạn 2 (NS, LG) → Giai đoạn 3 (TS, BH, BC) → Giai đoạn 4 (MK), mỗi module lặp lại đúng pattern đã quen ở bước 1-6 | Từ đây tốc độ sẽ nhanh dần vì pattern đã nằm trong tay |

Từ bước 1 trở đi là lúc "vừa học vừa làm" thật sự bắt đầu — mỗi bước nên là một buổi làm việc riêng, đi cùng Claude viết code thật cho đúng module đó, thay vì cố nhồi nhiều module cùng lúc.

---

<a id="12"></a>

## 12. Bảng thuật ngữ nhanh: Spring ↔ khái niệm chung

| Từ trong Spring | Tương đương ở framework/khái niệm chung bạn có thể đã biết |
|---|---|
| Bean | Một object được container quản lý (gần giống singleton service trong DI container của framework khác) |
| `@Component`/`@Service`/`@Repository`/`@Controller` | Cách đánh dấu "class này là một bean" — 4 tên khác nhau chỉ để rõ nghĩa theo tầng |
| ApplicationContext | IoC container — nơi giữ và nối mọi bean với nhau |
| Starter (`spring-boot-starter-*`) | Một "bộ" dependency đóng gói sẵn cho một nhu cầu cụ thể (web, DB, security...) |
| `@RestController` | Route handler / controller ở Express, Laravel, Django... |
| Repository (Spring Data JPA) | ORM model / DAO |
| DTO | Serializer/schema (kiểu như Pydantic schema ở FastAPI, hay Serializer ở Django REST Framework) |
| Filter (`OncePerRequestFilter`) | Middleware |
| `@Transactional` | Database transaction wrap quanh một method |
| Bean Validation (`@NotBlank`...) | Validation layer (giống Joi, Zod, Pydantic validator...) |
| Profile (`application-prod.yml`) | Environment config (giống `.env.production`) |
| Actuator | Health-check / metrics endpoint có sẵn |
| Embedded Tomcat | Web server tích hợp sẵn trong runtime (khác với PHP/Node cần Nginx/Apache riêng đứng trước) |

---

<a id="13"></a>

## 13. Nguồn tham khảo phiên bản

Số phiên bản Java/Spring Boot trong tài liệu này được tra cứu thực tế ngày 27/09/2026, không lấy từ trí nhớ huấn luyện của Claude (vì đây là loại thông tin thay đổi theo thời gian):

- [Spring Boot | endoflife.date](https://endoflife.date/spring-boot) — phiên bản Spring Boot 4.1.x hiện hành, yêu cầu Java tối thiểu, lịch hỗ trợ từng dòng
- [Spring Boot 4 & Spring Framework 7 – What's New | Baeldung](https://www.baeldung.com/spring-boot-4-spring-framework-7) — thay đổi chính so với Spring Boot 3 (Jackson 3.x, JUnit Jupiter, Jakarta EE 11...)
- [Spring Boot 4.0.0 available now | spring.io](https://spring.io/blog/2025/11/20/spring-boot-4-0-0-available-now/) — thông báo phát hành chính thức từ đội Spring
- [JDK Releases | javaalmanac.io](https://javaalmanac.io/jdk/) — lịch phát hành Java, xác nhận Java 25 là LTS mới nhất

Vì đây là thông tin có thể thay đổi, nếu bạn đọc tài liệu này sau một khoảng thời gian dài, nên kiểm tra lại số phiên bản mới nhất trên `start.spring.io` trước khi tạo project.
