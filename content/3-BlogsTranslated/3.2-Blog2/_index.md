---
title: "Blog 2"
 
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Modernize .NET applications at scale with AWS Transform for .NET

## Introduction

This blog post will walk you through a sample transformation using the new AWS Transform for .NET web experience. You’ll learn how to set up your environment, configure connections, execute the transformation process, and analyze results.

Organizations running .NET Framework applications face growing challenges with licensing costs, scalability, and security while continuing to rely on aging Windows-dependent software. While migrating these applications to the cloud is a step forward, it often falls short of unlocking their full potential for performance, scalability, and cost efficiency.

To address these challenges, AWS recently announced the general availability of AWS Transform for .NET, the first agentic AI service for modernizing .NET applications at scale. AWS Transform for .NET helps you to modernize Windows .NET applications to be Linux-ready up to four times faster than traditional methods and realize up to 40% savings in licensing costs. The experience is available as a unified web experience for large-scale transformations, and within your integrated development environment (IDE) for individual project and solution porting.

---

## Prerequisites

For this walk-through, ensure you have the following:

1. A standalone AWS account or a working setup of AWS Organizations
2. A working setup of AWS Identity and Access Management (IAM) identity center to federate into the web experience
3. AWS Transform enabled via the AWS management console. Refer to the AWS Transform User Guide.
4. An active GitHub account with administrative access.
5. The sample application forked into your GitHub repository (branch : **transform-blog**)
6. An AWS CodeConnection set up in your AWS account. Refer to the Developer Tools Console User Guide to create a connection to your GitHub account.

---

## Understanding the application

Bob’s Used Books Classic is a sample e-commerce application built using ASP.NET MVC targeting .NET Framework 4.8. The solution contains several projects

1. **Bookstore.Web**: The main ASP.NET MVC web application that serves as the user interface.
2. **Bookstore.Domain**: Core domain models and interfaces (distributed as a private NuGet package).
3. **Bookstore.Data**: Data access layer implementing repositories and services.
4. **Bookstore.Common**: Shared utilities and helper classes.
5. **Bookstore.Web.Tests** and **Bookstore.Domain.Tests**: Unit test projects.

The **Bookstore.Common** project is compiled into NuGet packages with different target frameworks and stored in a local directory (./nuget-packages).

1. **Common.1.0.0.nupkg**: Compatible with .NET Framework 4.8
2. **Common.2.0.0.nupkg**: Compatible with .NET 8.0

**Bookstore.Web** references the local Nuget package, **Bookstore.Common v1.0.0.** AWS Transform for .NET will scan your project files for private dependencies such as Bookstore.Common and prompt you to upload the packages. These packages help AWS Transform build your source code, resolve dependencies and address compatibility issues while migrating to cross-platform .NET.

---

## Walkthrough

### Step 1: Set up

Once you have met the prerequisites, log into the AWS Transform web experience portal using your identity center credentials. This unified web interface allows natural language interaction with AWS Transform, enabling collaborative execution of large-scale modernization projects.

#### 1. Create a workspace

Create a workspace to serve as your central hub for .NET modernization activities. To create a workspace, choose **Create Workspace** (Figure 1).

![.net1](/images/.net1.png)

AWS Transform creates a new workspace with a default name. Select the **Edit** link and enter a descriptive name for your workspace that reflects your .NET modernization project as shown in Figure 2 and 3.

![.net2](/images/.net2.png)

---

You’ll notice your initials in the top right corner of your workspace. To invite additional collaborators to your workspace, select the plus icon next to your initials (Figure 4).

Invite collaborators to the workspace
Figure 4: Invite collaborators

You can assign collaborators to one of the four roles – administrator, approver, contributor and view only (Figure 5). Select a role to match your team’s responsibilities and then click **Invite**.

Assign roles to collaborators
Figure 5: Assign roles

### Step 2: Transform .NET applications
#### 1. Create a .NET modernization job
Within your shared workspace, you can initiate transformation jobs by selecting **Create a job** (Figure 6).

Create a transformation job
Figure 6: Create a job

You will see a list of generative AI agents available from AWS Transform web experience. You can enter **.NET** in the chat to proceed (Figure 7).

Create a .NET modernization job
Figure 7: Create .NET job

AWS Transform provides default objectives and a job name for your .NET transformation. In this example, we’ll rename it to “DotNet-Blog-Job-1” as shown in Figure 8.

Rename the .NET job
Figure 8: Rename the .NET job

Verify the name change, then select **Create job** to create a .NET modernization job plan (Figure 9).

Confirm the configurations to create a .NET job
Figure 9: Confirm and create a .NET job

You will see a new pane with the job name – DotNet-Blog-Job-1 and the job plan under it, which is a guided step-by-step process that walks you through a .NET modernization (Figure 10).

Steps in a .NET Job plan
Figure 10: Job plan

#### 2. Set up source code connectors
Now, you will set up a connector to your source code repositories. The web experience for the .NET agent supports connecting to source code repositories from GitHub, Bitbucket, and GitLab enabling bulk transformation of .NET applications.

Select the task, **Connect source code repository** from the job plan. Under the Collaboration tab, enter the AWS account ID where you previously set up the CodeConnection. Select **Next** to continue (Figure 11).

Figure 11: Connect source code repository

On the next screen, provide the following information as shown in Figure 12:

**Connection ARN**: Copy and paste the AWS Resource Name (ARN) of the AWS CodeConnections set up in the prerequisite.

**Connector Name**: A descriptive name to identify the connector.

Figure 12: Enter connector details

Choose **Initiate connection creation** to proceed to the next step where you will be provided with a verification link as shown in Figure 13.

Figure 13: Copy verification link

Copy and send the verification link to the administrator of the AWS account. The administrator approves the connection request in the AWS management console by clicking **Approve** (Figure 14).

Figure 14: Approve connector

Once approved, return to the Collaboration tab to view the details of the connector including the Approved status (Figure 15).

Verify the connector status from collaboration tab
Figure 15: Validate the connector status

Next, select **Finalize connector** to complete the source code connector setup (Figure 16).

Finalize the connector to proceed
Figure 16: Finalize connector

#### 3. Discover resources for transformation

AWS Transform proceeds to the next step in your job plan, Discover resources for transformation. In the Worklog, you can see that AWS Transform is setting up resources to assess the repositories (Figure 17).

Discovery process tracking from Worklog tab
Figure 17: Initiate discovery – worklog

During the discovery process, AWS Transform performs a comprehensive analysis of your codebase. This includes assessing source code repositories, identifying .NET versions, project types, and mapping code and package dependencies (including private and public packages).

Throughout the discovery process, you can monitor real-time progress and review the detailed action log in the Worklog tab (Figure 18).

Detailed logs in the worklog tab
Figure 18: Detailed logs in worklog

#### 4. Preparing for transformation

Next, AWS Transform proceeds to generate a recommended transformation plan. Expand the Prepare for transformation section and select the data task “Confirm repositories to transform”.

In the Job details section of the Collaboration tab, you can select the following as shown in Figure 19:

- Target branch: Name of the target branch. Note this branch name. You will need it after the transformation is complete to review ported code.
- Target version: Leave the default value of .NET 8.0.
- Exclude .NET Standard projects from the transformation plan: Leave the default value of selected which excludes any .NET standard projects.
- Transform Model-View Controller (MVC) Razor Views to ASP.NET Core Razor: To port MVC Razor views to ASP.NET Core Razor views, leave the default value of selected.

Figure 19: Job details in the recommended plan

At this step, you can choose to either use the AWS Transform generated plan or customize the transformation plan to manually select the repositories (Figure 20).

Choose transformation plan
Figure 20: Choose a transformation plan

The AWS Transform generated plan intelligently orders repositories according to their last modification dates, dependency relationships, private package requirements, and the presence of supported project types.

Click the Download list (Figure 21) to obtain the JSON-formatted assessment report that provides in-depth metrics at both the repository and project levels, including lines of code, .NET project types, framework versions, and number of NuGet packages (both public and private). The plan also displays cross-dependent repositories, helping you map relationships across your applications and plan your large-scale .NET modernization effectively.

Download detailed assessment report
Figure 21: Download assessment report

#### 5. Customize the transformation plan

 Select Customize the transformation plan to proceed (Figure 22). You can either select repositories directly from the UI or download the repository JSON, modify it with your preferred selections, and upload the modified file.

Customize the transformation plan
Figure 22: Customize plan

Select the branch name, transform-blog for the Source branch and click the checkbox next to the repository – “bobs-used-bookstore-classic” (Figure 23). Upon selection, you’ll notice that a branch assessment is required.

Choose the source branch for transformation from the dropdown
Figure 23: Select branch

Click on Confirm repositories to proceed (Figure 24).

Confirm repositories to proceed to next step
Figure 24: Confirm repositories

AWS Transform will then assess the selected branch. You can track the assessment progress in the Worklog tab.

#### 6. Resolve package dependencies

 Many customers use NuGet packages from private sources for security and collaboration. AWS Transform for .NET detects NuGet package references from private sources and prompts you to provide those packages. This enables AWS Transform to build the source code and resolve build errors during transformation. The BookStore.Web project references the NuGet package – Bookstore.Common, from a local feed. On the Resolve package dependencies page, you will see the list of missing package dependencies as shown in Figure 25.

You can either:

- Use the provided PowerShell script to automatically retrieve missing packages across all selected repositories. Refer documentation for more details.
- Upload package files manually.

Figure 25: Missing package dependencies

To upload package files manually, choose Upload package files. In the file explorer popup , navigate to the nuget-packages folder and select Bookstore.Common.1.0.0.nupkg and Bookstore.Common.2.0.0.nupkg as shown in Figure 26 and choose Upload.

Upload package files manually via file explorer
Figure 26: Manually selected packages

In the Missing package dependencies table, wait for the Framework version status and Core version status to show Resolved (Figure 27).

Verify the package dependency status after upload
Figure 27: Resolved package dependencies

#### 7. Review and start transformation

Next step is to review and initiate the transformation process. Select Review and start transformation from the job plan.

On the Collaboration tab, verify the key configurations such as target branch destination, target version, and the transformation settings (Figure 28).

Review transformation job summary
Figure 28: Review job summary

Review the selected repositories, dependencies, and packages for transformation by scrolling down (Figure 29).

Review repositories and packages selected for transformation
Figure 29: Review repositories and packages

After reviewing the transformation plan, click Approve and start transformation (Figure 30).

Start transformation process 
Figure 30: Start transformation

AWS Transform begins executing the approved plan, automatically handling framework upgrades, dependencies, and code updates to align with modern .NET standards. You can monitor progress through the Worklog tab for detailed transformation steps with timestamps (Figure 31) or check the Dashboard tab for overall progress percentage and repository status summary (Figure 32).

Track transformation progress in Worklog
Figure 31: Track progress in Worklog

View transformation progress summary in dashboardFigure 32: View progress summary in Dashboard

#### 8. Review transformation results

After your transformation job completes successfully, you’ll receive an automated email notification (Figure 33).

 Email notification with deep links
Figure 33: Email notification with deep links

To review the transformation results, click the View job deep link in the email. This will take you directly to the Dashboard tab where you can examine the transformation summary (Figure 34).

Transformaton summary view in Dashboard tab
Figure 34: Transformation summary dashboard

You’ll see the overall status of your repository transformation, along with detailed information about each project within it. AWS Transform for .NET executes unit tests for the transformed code, displaying the results under Unit test status in the transformation summary.

Click on Download JSON to retrieve a detailed transformation summary (Figure 35).

Download detailed transformation summary
Figure 35: Retrieve detailed transformation summary

The detailed summary shows transformed repositories and their details, along with the status of the transformation actions performed for each project within a repository. It also provides a natural language transformation summary at the project level explaining key technical changes made to the codebase (Figure 36).

View detailed transformation summary in JSON format
Figure 36: Detailed transformation summary

#### 9. Review transformed code

To review the ported code, switch to the target branch from the notification email in your repository. Open BooksBookstoreClassic.sln in Visual Studio. Navigate to Bookstore.Web in Solution Explorer and check its Properties to confirm the Target framework is now .NET 8.0 (Figure 37).

Verify the Target framework version of .NET 8 in Visual Studio
Figure 37: Target framework is .NET 8.0

Using Solution Explorer, right click on Bookstore.Web and choose Manage NuGet packages to confirm that the version of Bookstore.Common is 2.0.0 (Figure 38).

Verify updated private Nuget package version 
Figure 38: Upgraded package reference

In Bookstore.Web project, open Controllers\AddressController.cs to confirm that the code references the newer .NET 8 Microsoft.AspNetCore.Mvc namespace (Figure 39).

Review updated namespace reference in Controllers\AddressController.cs
Figure 39: Updated namespace reference

In Bookstore.Web project, check Areas\Admin\Views\ReferenceData\CreateUpdate.cshtml line 18 for Html.GetEnumSelectList method as shown in Figure 40. This ASP.NET Core MVC method replaces the .NET Framework’s Html.GetSelectListForEnum.

Review the updated method calls in Razor views
Figure 40: Updated method calls in Razor code

#### 10. Run the application / Pending tasks

 The project originally used Autofac for dependency injection (see Bookstore.Web/App_Start/DependencyInjectionSetup.cs.bak). In this blog, you will add code in ASP.NET Core middleware to use the built-in dependency injection available on .NET 8 to replace Autofac’s functionality.

Replace the contents of Bookstore.Web/Program.cs with the following code:
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
To verify your transformed application:

1. In Visual Studio, press F5 or choose Debug, Start Debugging.
2. Verify the application in the browser (Figure 41).

Figure 41: Ported .NET 8 application running in browser

## Clean up

Follow these steps to clean up resources,

- Delete .NET job.
- Delete workspace.
- Delete CodeConnection to avoid potential future charges.
- Archive or delete transformation branches in your GitHub if no longer required.

## Conclusion

AWS Transform for .NET provides a powerful, agentic AI-driven solution for large-scale modernization of .NET Framework applications. This walkthrough demonstrates how the web experience simplifies modernization by streamlining discovery and assessment, offering intelligent transformation planning, and automating the upgrade to cross-platform .NET while maintaining business logic. The transformed Bob’s Used Books application, now running on .NET 8, benefits from cross-platform support, better performance, and lower operational costs.

Call to action – Begin your .NET Modernization journey with AWS Transform:

1. Explore AWS Transform for .NET to learn about features and benefits.
2. Try the interactive demo.
3. Visit the AWS Transform console to start modernizing your applications.
4. Check our blog for a guide on using AWS Transform for .NET in Visual Studio IDE.
 
 ---

AWS has significantly more services, and more features within those services, than any other cloud provider, making it faster, easier, and more cost effective to move your existing applications to the cloud and build nearly anything you can imagine. Give your Microsoft applications the infrastructure they need to drive the business outcomes you want. Visit our .NET on AWS and AWS Database blogs for additional guidance and options for your Microsoft workloads. Contact us to start your migration and modernization journey today.