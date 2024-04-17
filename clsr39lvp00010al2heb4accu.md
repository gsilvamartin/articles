---
title: "Integrating with Google Gemini: Using a Built .NET SDK"
datePublished: Sun Feb 18 2024 05:50:25 GMT+0000 (Coordinated Universal Time)
cuid: clsr39lvp00010al2heb4accu
slug: integrating-with-google-gemini-using-a-built-net-sdk
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708235661254/29ce6d8a-7111-4d1c-956b-98f786a75a6a.png
tags: csharp, net, sdk, gemini, google-gemini

---

## 🚀 Introduction

Google Gemini, renowned for its AI prowess, stands as a trailblazer in the realm of artificial intelligence. However, the absence of a dedicated .NET SDK presents a challenge for .NET developers. In this article, let's delve into bridging this gap with a custom .NET SDK tailored explicitly for Google Gemini.

## 🌟 Motivation

In the landscape of .NET development, the lack of an official SDK for Google Gemini can pose hurdles when interacting with the API. To overcome this, I embarked on creating a bespoke SDK, aiming to cater to the unique needs and functionalities of Google Gemini.

The drive behind this initiative stems from a desire to enrich the development experience for .NET developers engaging with Google Gemini. By encapsulating the intricacies of the Gemini API and furnishing a more idiomatic .NET interface, this SDK seeks to simplify the process of leveraging machine learning capabilities for content generation.

## 📦 How to Download?

Get started by installing the DotnetGeminiSDK NuGet package. Run the following command in the NuGet Package Manager Console:

`Install-Package DotnetGeminiSDK`

Or, if you prefer using the .NET CLI:

`dotnet add package DotnetGeminiSDK`

## 🔧 How to use?

To use the Gemini SDK, add the Gemini client to your service collection using GeminiServiceExtensions:

```csharp
using DotnetGeminiSDK;
using Microsoft.Extensions.DependencyInjection;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddGeminiClient(config =>
        {
            config.ApiKey = "YOUR_GOOGLE_GEMINI_API_KEY";
            config.ImageBaseUrl = "CURRENTLY_IMAGE_BASE_URL";
            config.TextBaseUrl = "CURRENTLY_IMAGE_BASE_URL";
        });
    }
}
```

When you incorporate the Gemini client, you can seamlessly inject it into your code for immediate use.

```csharp
using DotnetGeminiSDK.Client.Interfaces;
using Microsoft.Extensions.DependencyInjection;

public class YourClass
{
    private readonly IGeminiClient _geminiClient;

    public YourClass(IGeminiClient geminiClient)
    {
        _geminiClient = geminiClient;
    }

    public async Task Example()
    {
        var response = await _geminiClient.TextPrompt("Text for processing");
        // Process the response as needed
    }
}
```

## 🗝️ Existing Features

To demonstrate the use of the SDK, i will implement a simple API with two endpoints to showcase its versatility.

For using the SDK i've created a test api and put the GeminiClient in the constructor to receive it by dependency injection, you don't need to inject it again, when you call `services.AddGeminiClient()` it already inject it for you.

![Gemini Dependency Injection](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i11w5655kkplisgsz66z.png align="left")

## 📝 Text Prompt

For the text prompt, we have two options: sending a single message or multiple messages in batch. Additionally, we can define options for GenerationConfig and SafetySettings.

In this simple example, we will build a simple api where we receive the text input and use the `TextPrompt` method to call the Gemini.

![Gemini Text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/19y8s8slo8rojaxo9axl.png align="left")

As you see it's very simple, but you can customize it sending the optional parameters to method like `GenerationConfig` and `SafetySetting`.

Now let's test

![Prompt Request](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/18ryfz3faujh2xcg16se.png align="left")

**Success!**

As you see, we have the full Gemini response, so you can use it as needed.

## 🏞️ Image Prompt

In this method we need to send three parameters, the text to be considered, the image in `base64` or `byte[]` and the MimeType of the image using the SDK Enum: `ImageMimeType`, remembering that the formats accepted by Google are:

```plaintext
Jpg 
Jpeg 
Png 
Webp 
Heic 
Heif
```

To verify the capability of the image description feature, we've implemented a dedicated endpoint in our API. This endpoint allows you to submit an image along with a descriptive text, prompting Google Gemini to analyze the image and provide a description.

![Api](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/23zdl30txueczjcroqsu.png align="left")

As the image above illustrates, I need to send the image in base64 format, its mimetype, and the message to Gemini. Therefore, let's choose a `Jpeg` image for this test.

Let's use this cute Shiba Inu photo to check if it works

![Shiba Inu](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w7vffywhs9i44oyrpq2e.png align="left")

### Test Results

As we can see in bellow image, we have requested to Gemini to tell which kind of dog we have in the image, and he answered Shiba Inu.

![It works!!](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c9den6zympwpc3o3c5mj.png align="left")

**Success!**

## ✅ Conclusion

In this article, I demonstrated how to use Gemini in a simple and quick manner. It's important to note that the SDK is experimental and may have issues or lack some features, but it is entirely usable. Feel free to explore, experiment, and leverage the capabilities provided by this SDK, adapting it as needed to meet your specific requirements.

Thanks for reading!

### GitHub SDK Link:

[DotnetGeminiSDK Repository](https://github.com/gsilvamartin/dotnet-gemini-sdk)