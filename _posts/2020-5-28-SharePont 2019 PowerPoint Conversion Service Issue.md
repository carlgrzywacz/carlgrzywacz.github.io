---
layout: post
title: SharePont 2019 PowerPoint Conversion Service Issue
---


Problem: You are trying to use the PowerPoint Conversion Service in SharePoint 2019 following the code sample from <a href="https://docs.microsoft.com/en-us/sharepoint/dev/general-development/powerpoint-automation-services-in-sharepoint">this documentation</a>.  The code throws the following error: "Microsoft.Office.Server.PowerPoint.Conversion.ConversionException: Could not connect to the conversion service." 

Solution: SharePoint 2019 has a bug that does not allow the PowerPoint Conversion Service to work in all scenarios.  The fix for this is to make the following web.config change on all servers in the farm running the PowerPoint Conversion Service.
	• Open C:\Program Files\CommonFiles\MicrosoftShared\WebServerExtensions\16\WebServices\PowerPointConversion\web.config
	• Add the following on line 3 (after <configuration>and before <system.serviceModel>) :-<appSettings><add key="ConfigAdapterAssemblyQualifiedTypeName" value="Microsoft.Office.ConversionServices.Local.Environment.ConfigAdapter,Microsoft.Office.Word.Server,Version=16.0.0.0,Culture=neutral,PublicKeyToken=71e9bce111e9429c"/>
</appSettings>

Full error stack trace: Microsoft.Office.Server.PowerPoint.Conversion.ConversionException: Could not connect to the conversion service. --- ConversionError: ServiceUnavailable ---> System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: The method or operation is not implemented. (Fault Detail is equal to An ExceptionDetail, likely created by IncludeExceptionDetailInFaults=true, whose value is:
System.NotImplementedException: The method or operation is not implemented.
at Microsoft.Office.ServiceInfrastructure.Runtime.EnvironmentAdapters.ConfigAdapterBase.TryGetOverride[T](String settingName, T& value)
at Microsoft.Office.ServiceInfrastructure.Runtime.Settings.OsiBrsSetting`1.get_CurrentValue()
at Microsoft.Office.Web.Common.TimerThresholdLogger..ctor(String message)
at Microsoft.Office.Web.Common.OrderedLock.AcquireLock(Int32 millisecondsTimeout, String caller)
at Microsoft.Office.Web.Conversion.Framework.AppManager.LookForAnyOldUninitializedWorkers()
at Microsoft.Office.Web.Conversion.Framework.AppManager.BeginProcessRequest(ConversionSession session, Int32& queuePosition, AsyncCallback callback, Object state)
at Microsoft.Office.Server.PowerPoint.Conversion.ConversionService.BeginConvert(StreamConversionRequest request, AsyncCallback callback, Object state)
at AsyncInvokeBe...).
--- End of inner exception stack trace ---
at Microsoft.Office.Web.Common.AsyncResult`1.WaitForCompletion()
at Microsoft.Office.Server.PowerPoint.Conversion.Request.EndConvert(IAsyncResult result)