---
title: "Blog 2"
 
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Hiện đại hóa các ứng dụng .NET trên quy mô lớn với AWS Transform for .NET

## Giới thiệu

Bài viết này sẽ hướng dẫn bạn từng bước thực hiện quy trình chuyển đổi mẫu sử dụng giao diện web mới của AWS Transform for .NET. Bạn sẽ học cách thiết lập môi trường, cấu hình kết nối, thực thi quy trình chuyển đổi và phân tích kết quả.

Các tổ chức đang vận hành ứng dụng .NET Framework hiện phải đối mặt với những thách thức ngày càng lớn về chi phí bản quyền, khả năng mở rộng và bảo mật, trong khi vẫn phụ thuộc vào phần mềm lỗi thời chạy trên nền tảng Windows. Dù việc di chuyển các ứng dụng này lên đám mây là một bước tiến quan trọng, giải pháp này thường chưa khai thác hết tiềm năng về hiệu suất, khả năng mở rộng và hiệu quả chi phí.

Để giải quyết những thách thức này, AWS gần đây đã công bố tính khả dụng chung của AWS Transform for .NET, dịch vụ AI tự trị đầu tiên để hiện đại hóa ứng dụng .NET trên quy mô lớn. AWS Transform for .NET giúp bạn hiện đại hóa ứng dụng Windows .NET để sẵn sàng chạy trên Linux nhanh hơn đến bốn lần so với các phương pháp truyền thống và tiết kiệm đến 40% chi phí cấp phép. Trải nghiệm này có sẵn dưới dạng một giao diện web thống nhất cho các quy trình chuyển đổi quy mô lớn, và trong môi trường phát triển tích hợp (IDE) của bạn để chuyển đổi các dự án và giải pháp riêng lẻ.


---

## Điều kiện tiên quyết

For this walk-through, ensure you have the following:

Để thực hiện theo hướng dẫn này, hãy đảm bảo bạn có những yếu tố sau:

1. Một tài khoản AWS độc lập hoặc một thiết lập AWS Organizations đang hoạt động
2. Một thiết lập AWS Identity and Access Management (IAM) Identity Center đang hoạt động để liên kết đăng nhập vào trải nghiệm web
3. AWS Transform đã được kích hoạt thông qua AWS Management Console. Tham khảo Hướng dẫn sử dụng AWS Transform.
4. Một tài khoản GitHub đang hoạt động với quyền truy cập quản trị.
5. Ứng dụng mẫu đã được fork vào kho lưu trữ GitHub của bạn (nhánh: transform-blog)
6. AWS CodeConnection đã được thiết lập trong tài khoản AWS của bạn. Tham khảo Hướng dẫn sử dụng Developer Tools Console để tạo kết nối đến tài khoản GitHub của bạn.

---

## Tìm hiểu về ứng dụng

Bob’s Used Books Classic là một ứng dụng thương mại điện tử mẫu được xây dựng bằng ASP.NET MVC mục tiêu .NET Framework 4.8. Giải pháp bao gồm một số dự án sau:

1. **Bookstore.Web**: Ứng dụng web ASP.NET MVC chính đóng vai trò giao diện người dùng.
2. **Bookstore.Domain**: Các mô hình và giao diện miền cốt lõi (được phân phối dưới dạng gói NuGet riêng tư).
3. **Bookstore.Data**: Lớp truy cập dữ liệu triển khai các kho lưu trữ và dịch vụ.
4. **Bookstore.Common**: Các tiện ích và lớp trợ giúp dùng chung.
5. **Bookstore.Web.Tests** và **Bookstore.Domain.Tests**: Các dự án kiểm thử đơn vị.

Dự án **Bookstore.Common** được biên dịch thành các gói NuGet với các framework mục tiêu khác nhau và được lưu trữ trong một thư mục cục bộ (./nuget-packages).

1. **Common.1.0.0.nupkg**: Tương thích với .NET Framework 4.8
2. **Common.2.0.0.nupkg**: Tương thích với .NET 8.0

**Bookstore.Web** tham chiếu đến gói Nuget cục bộ, **Bookstore.Common v1.0.0.** AWS Transform for .NET sẽ quét các tệp dự án của bạn để tìm các phụ thuộc riêng tư như Bookstore.Common và nhắc bạn tải lên các gói này. Các gói này giúp AWS Transform xây dựng mã nguồn, giải quyết các phụ thuộc và xử lý các vấn đề tương thích trong quá trình di chuyển sang .NET đa nền tảng.


---

## Hướng dẫn từng bước

### Bước 1: Thiết lập

Sau khi đã đáp ứng các điều kiện tiên quyết, hãy đăng nhập vào cổng trải nghiệm web AWS Transform bằng thông tin đăng nhập Identity Center của bạn. Giao diện web thống nhất này cho phép tương tác bằng ngôn ngữ tự nhiên với AWS Transform, cho phép thực hiện hợp tác các dự án hiện đại hóa quy mô lớn.
 modernization projects.

#### 1. Tạo một workspace

Tạo một workspace để đóng vai trò là trung tâm cho các hoạt động hiện đại hóa .NET. Để tạo workspace, hãy chọn **Create Workspace** (Hình 1).

![.net1](/images/.net1.png)

AWS Transform sẽ tạo một workspace mới với tên mặc định. Chọn liên kết **Edit** và nhập một tên mô tả cho workspace của bạn để phản ánh dự án hiện đại hóa .NET của bạn như được minh họa trong Hình 2 và 3.

![.net2](/images/.net2.png)

---

Bạn sẽ nhận thấy avatar của mình ở góc trên bên phải workspace. Để mời những cộng tác viên bổ sung vào workspace, hãy chọn biểu tượng dấu cộng (+) bên cạnh collaborators (Hình 4).

Hình 4: Mời cộng tác viên

Bạn có thể gán cho các cộng tác viên một trong bốn vai trò - administrator, approver, contributor và view only (Hình 5). Chọn một vai trò phù hợp với trách nhiệm của nhóm bạn và sau đó nhấp vào **Invite**.

Hình 5: Chọn vai trò

### Bước 2: Chuyển đổi ứng dụng .NET

#### 1. Tạo một job hiện đại hóa .NET

Trong workspace được chia sẻ của bạn, bạn có thể bắt đầu các job chuyển đổi bằng cách chọn **Create a job** (Figure 6).

Hình 6: Tạo một job

Bạn sẽ thấy một danh sách các tác nhân AI sáng tạo có sẵn từ trải nghiệm web AWS Transform. Bạn có thể nhập .NET vào khung trò chuyện để tiếp tục (Hình 7).

Hình 7: Tạo job .NET

AWS Transform cung cấp sẵn các mục tiêu và tên job mặc định cho quá trình chuyển đổi .NET của bạn. Trong ví dụ này, chúng ta sẽ đổi tên nó thành "DotNet-Blog-Job-1" như trong Hình 8.

Hình 8: Đổi tên job .NET

Xác nhận thay đổi tên, sau đó chọn **Create job** để tạo một job plan hiện đại hóa .NET (Hình 9).

Figure 9: Confirm and create a .NET job

Bạn sẽ thấy một ngăn mới với tên job - DotNet-Blog-Job-1 và job plan bên dưới, đây là một quy trình hướng dẫn từng bước đưa bạn qua quá trình hiện đại hóa .NET (Hình 10).

Figure 10: Job plan

#### 2. Thiết lập kết nối mã nguồn

Bây giờ, bạn sẽ thiết lập một kết nối đến các kho lưu trữ mã nguồn của mình. Trải nghiệm web cho tác nhân .NET hỗ trợ kết nối đến các kho lưu trữ mã nguồn từ GitHub, Bitbucket và GitLab, cho phép chuyển đổi hàng loạt các ứng dụng .NET.

Chọn tác vụ **Connect source code repository** từ kế hoạch job. Trong tab Collaboration, hãy nhập AWS Account ID nơi bạn đã thiết lập CodeConnection trước đó. Chọn Next để tiếp tục (Hình 11).

Figure 11: Connect source code repository

Ở màn hình tiếp theo, hãy cung cấp các thông tin sau như trong Hình 12:

- **Connection ARN**: Sao chép và dán AWS Resource Name (ARN) của AWS CodeConnections đã được thiết lập trong phần điều kiện tiên quyết.

- **Connector Name**: Một tên mô tả để nhận diện connector.

Figure 12: Enter connector details

Chọn **Initiate connection creation** để chuyển sang bước tiếp theo, nơi bạn sẽ được cung cấp một liên kết xác minh như trong Hình 13.

Figure 13: Copy verification link

Hãy sao chép và gửi liên kết xác minh này cho quản trị viên của tài khoản AWS. Quản trị viên phê duyệt yêu cầu kết nối trong AWS Management Console bằng cách nhấp vào **Approve** (Figure 14).

Figure 14: Approve connector

Sau khi được phê duyệt, hãy quay lại tab *Collaboration* để xem chi tiết của connector, bao gồm trạng thái *Approved* (Hình 15).

Verify the connector status from collaboration tab
Figure 15: Validate the connector status

Tiếp theo, chọn Finalize connector để hoàn tất việc thiết lập connector mã nguồn (Hình 16).

Finalize the connector to proceed
Figure 16: Finalize connector

#### 3. Khám phá tài nguyên để chuyển đổi

AWS Transform chuyển sang bước tiếp theo trong kế hoạch job của bạn, Discover resources for transformation. Trong Worklog, bạn có thể thấy AWS Transform đang thiết lập các tài nguyên để đánh giá các kho lưu trữ (Hình 17)..

Discovery process tracking from Worklog tab
Figure 17: Initiate discovery – worklog

Trong quá trình khám phá, AWS Transform thực hiện phân tích toàn diện mã nguồn của bạn. Điều này bao gồm việc đánh giá các kho lưu trữ mã nguồn, xác định phiên bản .NET, loại dự án và lập bản đồ các phụ thuộc về mã và gói (bao gồm cả các gói riêng tư và công khai).

Xuyên suốt quá trình khám phá, bạn có thể theo dõi tiến độ thời gian thực và xem lại nhật ký hành động chi tiết trong tab Worklog (Hình 18).

Detailed logs in the worklog tab
Figure 18: Detailed logs in worklog

#### 4. Chuẩn bị cho quá trình chuyển đổi

Tiếp theo, AWS Transform tiến hành tạo một kế hoạch chuyển đổi được đề xuất. Hãy mở rộng phần Prepare for transformation và chọn tác vụ dữ liệu "Confirm repositories to transform".

Trong phần Job details của tab Collaboration, bạn có thể chọn các mục sau như trong Hình 19:

- Target branch: Tên của nhánh mục tiêu. Hãy ghi nhớ tên nhánh này. Bạn sẽ cần nó sau khi quá trình chuyển đổi hoàn tất để xem xét mã đã được chuyển đổi.
- Target version: Giữ nguyên giá trị mặc định là .NET 8.0.
- Exclude .NET Standard projects from the transformation plan: Giữ nguyên giá trị mặc định đã được chọn, nhằm loại trừ bất kỳ dự án .NET Standard nào.
- Transform Model-View Controller (MVC) Razor Views to ASP.NET Core Razor: Để chuyển đổi MVC Razor views sang ASP.NET Core Razor views, hãy giữ nguyên giá trị mặc định đã được chọn.

Figure 19: Job details in the recommended plan

Ở bước này, bạn có thể chọn sử dụng kế hoạch do AWS Transform tạo ra hoặc tùy chỉnh kế hoạch chuyển đổi để chọn thủ công các kho lưu trữ (Hình 20).

Choose transformation plan
Figure 20: Choose a transformation plan

Kế hoạch được tạo bởi AWS Transform sẽ sắp xếp các kho lưu trữ một cách thông minh theo ngày sửa đổi lần cuối, mối quan hệ phụ thuộc, yêu cầu về gói riêng tư và sự hiện diện của các loại dự án được hỗ trợ.

Nhấp vào Download list (Hình 21) để tải về báo cáo đánh giá ở định dạng JSON, cung cấp các số liệu chi tiết ở cả cấp độ kho lưu trữ và dự án, bao gồm số dòng mã, loại dự án .NET, phiên bản framework và số lượng gói NuGet (cả công khai và riêng tư). Kế hoạch cũng hiển thị các kho lưu trữ có sự phụ thuộc chéo, giúp bạn ánh xạ các mối quan hệ xuyên suốt các ứng dụng và lập kế hoạch hiện đại hóa .NET quy mô lớn một cách hiệu quả.


Download detailed assessment report
Figure 21: Download assessment report

#### 5. CTùy chỉnh kế hoạch chuyển đổi

 Chọn Customize the transformation plan để tiếp tục (Hình 22). Bạn có thể chọn trực tiếp các kho lưu trữ từ giao diện người dùng hoặc tải xuống tệp JSON của kho lưu trữ, sửa đổi nó với các lựa chọn ưu tiên của bạn và tải lên tệp đã sửa đổi.

Customize the transformation plan
Figure 22: Customize plan

Chọn tên nhánh, transform-blog cho Source branch và nhấp vào hộp kiểm bên cạnh kho lưu trữ – "bobs-used-bookstore-classic" (Hình 23). Sau khi chọn, bạn sẽ nhận thấy rằng cần phải có đánh giá nhánh.

Choose the source branch for transformation from the dropdown
Figure 23: Select branch

Nhấp vào Confirm repositories để tiếp tục (Hình 24).

Confirm repositories to proceed to next step
Figure 24: Confirm repositories

AWS Transform sau đó sẽ đánh giá nhánh đã chọn. Bạn có thể theo dõi tiến trình đánh giá trong tab Worklog.

#### 6. Giải quyết các phụ thuộc gói

Nhiều khách hàng sử dụng các gói NuGet từ các nguồn riêng tư để bảo mật và hợp tác. AWS Transform for .NET phát hiện các tham chiếu gói NuGet từ các nguồn riêng tư và nhắc bạn cung cấp những gói đó. Điều này cho phép AWS Transform xây dựng mã nguồn và giải quyết các lỗi build trong quá trình chuyển đổi. Dự án BookStore.Web tham chiếu đến gói NuGet – Bookstore.Common, từ một nguồn cấp dữ liệu cục bộ. Trên trang Resolve package dependencies, bạn sẽ thấy danh sách các phụ thuộc gói bị thiếu như trong Hình 25.

Bạn có thể:

- Sử dụng script PowerShell được cung cấp để tự động truy xuất các gói bị thiếu trên tất cả các kho lưu trữ đã chọn. Tham khảo tài liệu để biết thêm chi tiết.
- Tải lên các tệp gói theo cách thủ công.


Figure 25: Missing package dependencies

Để tải lên các tệp gói theo cách thủ công, hãy chọn Upload package files. Trong cửa sổ bật lên của trình khám phá tệp, điều hướng đến thư mục nuget-packages và chọn Bookstore.Common.1.0.0.nupkg và Bookstore.Common.2.0.0.nupkg như trong Hình 26 và chọn Upload.

Upload package files manually via file explorer
Figure 26: Manually selected packages

Trong bảng Missing package dependencies, hãy chờ trạng thái Framework version và Core version hiển thị Resolved (Hình 27).

Verify the package dependency status after upload
Figure 27: Resolved package dependencies

#### 7. Xem xét và bắt đầu chuyển đổi

Bước tiếp theo là xem xét và bắt đầu quy trình chuyển đổi.
Chọn Review and start transformation từ kế hoạch job.

Trên tab Collaboration, hãy xác minh các cấu hình chính như đích đến nhánh mục tiêu, phiên bản mục tiêu và các cài đặt chuyển đổi (Hình 28).

Review transformation job summary
Figure 28: Review job summary

Xem xét các kho lưu trữ, phụ thuộc và gói đã chọn để chuyển đổi bằng cách cuộn xuống (Hình 29).

Sau khi xem xét kế hoạch chuyển đổi, hãy nhấp vào Approve and start transformation (Hình 30).

Start transformation process 
Figure 30: Start transformation

AWS Transform bắt đầu thực thi kế hoạch đã được phê duyệt, tự động xử lý các nâng cấp framework, phụ thuộc và cập nhật mã để phù hợp với các tiêu chuẩn .NET hiện đại. Bạn có thể theo dõi tiến trình thông qua tab Worklog để xem các bước chuyển đổi chi tiết với dấu thời gian (Hình 31) hoặc kiểm tra tab Dashboard để xem tổng quan phần trăm tiến độ và trạng thái kho lưu trữ (Hình 32).

Track transformation progress in Worklog
Figure 31: Track progress in Worklog

View transformation progress summary in dashboardFigure 32: View progress summary in Dashboard

#### 8. Xem xét kết quả chuyển đổi

Sau khi job chuyển đổi của bạn hoàn thành thành công, bạn sẽ nhận được một thông báo email tự động (Hình 33).

 Email notification with deep links
Figure 33: Email notification with deep links

Để xem xét kết quả chuyển đổi, hãy nhấp vào liên kết sâu View job trong email. Thao tác này sẽ đưa bạn trực tiếp đến tab Dashboard, nơi bạn có thể kiểm tra bản tóm tắt chuyển đổi (Hình 34).

Transformaton summary view in Dashboard tab
Figure 34: Transformation summary dashboard

Bạn sẽ thấy trạng thái tổng thể của quá trình chuyển đổi kho lưu trữ, cùng với thông tin chi tiết về từng dự án trong đó. AWS Transform for .NET thực thi các kiểm thử đơn vị cho mã đã chuyển đổi và hiển thị kết quả trong mục Unit test status ở bản tóm tắt chuyển đổi.

Click on Download JSON to retrieve a detailed transformation summary (Figure 35).

Download detailed transformation summary
Figure 35: Retrieve detailed transformation summary

Bản tóm tắt chi tiết hiển thị các kho lưu trữ đã được chuyển đổi và thông tin chi tiết của chúng, cùng với trạng thái của các hành động chuyển đổi được thực hiện cho từng dự án trong một kho lưu trữ. Nó cũng cung cấp một bản tóm tắt chuyển đổi bằng ngôn ngữ tự nhiên ở cấp độ dự án, giải thích các thay đổi kỹ thuật chính được thực hiện trên mã nguồn (Hình 36).

View detailed transformation summary in JSON format
Figure 36: Detailed transformation summary

#### 9. Xem xét mã đã chuyển đổi

Để xem xét mã đã được chuyển đổi, hãy chuyển sang nhánh mục tiêu được thông báo trong email từ kho lưu trữ của bạn. Mở BooksBookstoreClassic.sln trong Visual Studio. Điều hướng đến Bookstore.Web trong Solution Explorer và kiểm tra Properties của nó để xác nhận Target framework bây giờ là .NET 8.0 (Hình 37).

Verify the Target framework version of .NET 8 in Visual Studio
Figure 37: Target framework is .NET 8.0

Sử dụng Solution Explorer, nhấp chuột phải vào Bookstore.Web và chọn Manage NuGet Packages để xác nhận rằng phiên bản của Bookstore.Common là 2.0.0 (Hình 38).

Verify updated private Nuget package version 
Figure 38: Upgraded package reference

Trong dự án Bookstore.Web, mở Controllers\AddressController.cs để xác nhận rằng mã tham chiếu đến namespace Microsoft.AspNetCore.Mvc mới hơn của .NET 8 (Hình 39).

Review updated namespace reference in Controllers\AddressController.cs
Figure 39: Updated namespace reference

Trong dự án Bookstore.Web, kiểm tra dòng 18 trong tệp Areas\Admin\Views\ReferenceData\CreateUpdate.cshtml để tìm phương thức Html.GetEnumSelectList như trong Hình 40. Phương thức ASP.NET Core MVC này thay thế cho Html.GetSelectListForEnum của .NET Framework.

Review the updated method calls in Razor views
Figure 40: Updated method calls in Razor code

#### 10. Chạy ứng dụng / Các tác vụ còn lại

Dự án ban đầu sử dụng Autofac cho dependency injection (xem Bookstore.Web/App_Start/DependencyInjectionSetup.cs.bak). Trong blog này, bạn sẽ thêm mã vào middleware ASP.NET Core để sử dụng dependency injection được tích hợp sẵn trong .NET 8 để thay thế chức năng của Autofac.

Thay thế nội dung của Bookstore.Web/Program.cs bằng đoạn mã sau:
```cs
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using System;
using System.Configuration;
using System.Data.Entity;
using System.Linq;
using System.Collections.Generic;

namespace Bookstore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);

            // Apply environment settings from AppSettings
            builder.Configuration.GetSection("AppSettings")
                .GetChildren()
                .Where(setting => setting.Key == "Environment")
                .ToList()
                .ForEach(setting => builder.Environment.EnvironmentName = setting.Value);

            // Store configuration in static ConfigurationManager
            ConfigurationManager.Configuration = builder.Configuration;

            // Add services to the container
            builder.Services.AddControllersWithViews()
                .AddRazorOptions(options => {
                    // Add area view location formats
                    options.ViewLocationFormats.Add("/Areas/{2}/Views/{1}/{0}.cshtml");
                    options.ViewLocationFormats.Add("/Areas/{2}/Views/Shared/{0}.cshtml");
                    options.AreaViewLocationFormats.Add("/Areas/{2}/Views/{1}/{0}.cshtml");
                    options.AreaViewLocationFormats.Add("/Areas/{2}/Views/Shared/{0}.cshtml");
                });
            
            AddServices(builder.Services, builder);

            var app = builder.Build();

            // Configure the HTTP request pipeline
            if (app.Environment.IsDevelopment())
                app.UseDeveloperExceptionPage();
            else
            {
                app.UseExceptionHandler("/Home/Error");
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();
            app.UseRouting();
            app.UseAuthorization();

            // Global exception handler
            app.Use(async (context, next) => {
                try { await next.Invoke(); }
                catch (Exception ex)
                {
                    context.RequestServices.GetRequiredService<ILogger<Program>>()
                        .LogError(ex, "An unhandled exception occurred");
                    throw;
                }
            });

            // Register routes
            app.MapControllerRoute(
                name: "default",
                pattern: "{controller=Home}/{action=Index}/{id?}");
                
            app.MapControllerRoute(
                name: "areas",
                pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

            // Entity Framework configuration
            Database.SetInitializer(new CreateDatabaseIfNotExists<DbContext>());

            app.Run();
        }

        private static void AddServices(IServiceCollection services, WebApplicationBuilder builder)
        {
            var connectionString = builder.Configuration.GetConnectionString("BookstoreDatabaseConnection");
            
            // Register application services
            services.AddTransient<Data.ApplicationDbContext>(s => new Data.ApplicationDbContext(connectionString));
            services.AddControllersWithViews();
            
            // Domain services
            services.AddTransient<Domain.Books.IBookService, Domain.Books.BookService>();
            services.AddTransient<Domain.Orders.IOrderService, Domain.Orders.OrderService>();
            services.AddTransient<Domain.ReferenceData.IReferenceDataService, Domain.ReferenceData.ReferenceDataService>();
            services.AddTransient<Domain.Offers.IOfferService, Domain.Offers.OfferService>();
            services.AddTransient<Domain.Customers.ICustomerService, Domain.Customers.CustomerService>();
            services.AddTransient<Domain.Addresses.IAddressService, Domain.Addresses.AddressService>();
            services.AddTransient<Domain.Carts.IShoppingCartService, Domain.Carts.ShoppingCartService>();
            services.AddTransient<Domain.IImageResizeService, Data.ImageResizeService.ImageResizeService>();

            // Repositories
            services.AddTransient<Domain.Customers.ICustomerRepository, Data.Repositories.CustomerRepository>();
            services.AddTransient<Domain.Addresses.IAddressRepository, Data.Repositories.AddressRepository>();
            services.AddTransient<Domain.Books.IBookRepository, Data.Repositories.BookRepository>();
            services.AddTransient<Domain.Offers.IOfferRepository, Data.Repositories.OfferRepository>();
            services.AddTransient<Domain.Carts.IShoppingCartRepository, Data.Repositories.ShoppingCartRepository>();
            services.AddTransient<Domain.Orders.IOrderRepository, Data.Repositories.OrderRepository>();
            services.AddTransient<Domain.ReferenceData.IReferenceDataRepository, Data.Repositories.ReferenceDataRepository>();

            // Configure services based on configuration
            ConfigureFileService(services, builder);
            ConfigureImageValidationService(services, builder);
        }

        private static void ConfigureFileService(IServiceCollection services, WebApplicationBuilder builder)
        {
            if (builder.Configuration["Services:FileService"] == "aws")
            {
                services.AddTransient<Amazon.S3.IAmazonS3, Amazon.S3.AmazonS3Client>();
                services.AddTransient<Domain.IFileService, Data.FileServices.S3FileService>();
            }
            else
            {
                services.AddTransient<Domain.IFileService, Data.FileServices.LocalFileService>(
                    s => new Data.FileServices.LocalFileService(builder.Environment.WebRootPath));
            }
        }

        private static void ConfigureImageValidationService(IServiceCollection services, WebApplicationBuilder builder)
        {
            if (builder.Configuration["Services:ImageValidationService"] == "aws")
            {
                services.AddTransient<Amazon.Rekognition.IAmazonRekognition, Amazon.Rekognition.AmazonRekognitionClient>();
                services.AddTransient<Domain.IImageValidationService, Data.ImageValidationServices.RekognitionImageValidationService>();
            }
            else
            {
                services.AddTransient<Domain.IImageValidationService, Data.ImageValidationServices.LocalImageValidationService>();
            }
        }
    }

    public class ConfigurationManager
    {
        public static IConfiguration Configuration { get; set; }
    }
}
```

Để xác minh ứng dụng đã chuyển đổi của bạn:

1. Trong Visual Studio, nhấn F5 hoặc chọn Debug, Start Debugging.
2. Xác minh ứng dụng trong trình duyệt (Hình 41).

Figure 41: Ported .NET 8 application running in browser

## Dọn dẹp tài nguyên

Hãy làm theo các bước sau để dọn dẹp tài nguyên:

- Xóa job .NET.
- Xóa workspace.
- Xóa CodeConnection để tránh các chi phí tiềm ẩn trong tương lai.
- Lưu trữ hoặc xóa các nhánh chuyển đổi trong GitHub của bạn nếu không còn cần thiết.

## Kết luận

AWS Transform for .NET cung cấp một giải pháp mạnh mẽ, được dẫn dắt bởi AI tự trị cho quá trình hiện đại hóa quy mô lớn các ứng dụng .NET Framework. Hướng dẫn này minh họa cách trải nghiệm web đơn giản hóa việc hiện đại hóa bằng cách hợp lý hóa việc khám phá và đánh giá, đưa ra kế hoạch chuyển đổi thông minh và tự động hóa việc nâng cấp lên .NET đa nền tảng trong khi vẫn duy trì logic nghiệp vụ. Ứng dụng Bob’s Used Books sau khi chuyển đổi, giờ chạy trên .NET 8, được hưởng lợi từ hỗ trợ đa nền tảng, hiệu suất tốt hơn và chi phí vận hành thấp hơn.

Lời kêu gọi hành động – Bắt đầu hành trình Hiện đại hóa .NET của bạn với AWS Transform:

1. Khám phá AWS Transform for .NET để tìm hiểu về các tính năng và lợi ích.
2. Dùng thử bản demo tương tác.
3. Truy cập bảng điều khiển AWS Transform console để bắt đầu hiện đại hóa ứng dụng của bạn.
4. Kiểm tra blog của chúng tôi để xem hướng dẫn sử dụng AWS Transform for .NET trong Visual Studio IDE.

 ---

AWS sở hữu đa dạng dịch vụ vượt trội cùng những tính năng chuyên sâu mà không nhà cung cấp đám mây nào có được, giúp bạn di chuyển ứng dụng lên cloud nhanh chóng, dễ dàng và tối ưu chi phí, đồng thời hiện thực hóa mọi ý tưởng sáng tạo. Hãy trang bị nền tảng hạ tầng vững chắc cho các ứng dụng Microsoft của bạn để đạt được những mục tiêu kinh doanh kỳ vọng. Để khám phá thêm các hướng dẫn chi tiết và giải pháp tối ưu cho khối workload Microsoft của bạn, mời bạn ghé thăm các blog chuyên sâu .NET on AWS và AWS Database của chúng tôi. Hãy kết nối với đội ngũ chuyên gia AWS ngay hôm nay để bắt đầu hành trình chuyển đổi và hiện đại hóa ứng dụng của bạn.
