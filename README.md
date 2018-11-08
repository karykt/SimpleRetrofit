# Simple Retrofit Tutorial
This project is a simple step-by-step tutorial to create an Android application that uses the Retrofit library to retrieve and upload data (JSON or any other structured data) via a REST based web service.

## What is Retrofit?
Retrofit is a REST type-safe HTTP Client for Java and Android. Created by Square inc under Apache 2.0 license it turns your HTTP API into a Java interface. 
Retrofit makes relatively easy to retrieve and upload JSON (or other structured data) via a REST based webservice. In Retrofit you configure which converter is used for the data serialization.

## Getting Started

Let's get started by first creating a new project.

**1.Create a new project:**
Go to File ⇒ New Project. When it prompts you to select the default activity, select Empty Activity and proceed.

**2.Add dependencies:** 
Add this library in your build.gradle file in app module.

compile ‘com.squareup.retrofit2:retrofit:2.3.0’

**3.Add Internet Permission in the AndroidManifest.xml:** 
Define Internet permission in our Manifest file as showed bellow

