# Blazored LocalStorage - Secure
A library to provide access to local storage in Blazor applications

Only do this on Blazor Server, it will not be secure from Blazor Web Assembly.

### Setup

You will need to register the local storage services with the service collection in your _Startup.cs_ file in Blazor Server.

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddBlazoredLocalStorage();
}
``` 


### Configuration

The local storage provides options that can be modified by you at registration in your _Startup.cs_ file in Blazor Server.


```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddBlazoredLocalStorage(config =>
        config.JsonSerializerOptions.WriteIndented = true);
}
```


### Usage (Blazor Server)

**NOTE:** Due to pre-rendering in Blazor Server you can't perform any JS interop until the `OnAfterRender` lifecycle method.

```c#
@inject Blazored.LocalStorage.ILocalStorageService localStorage

@code {

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        await localStorage.SetItemAsync("name", "John Smith");
        var name = await localStorage.GetItemAsync<string>("name");
    }

}
```

The APIs available are:

- asynchronous via `ILocalStorageService`:
  - SetItemAsync()
  - GetItemAsync()
  - RemoveItemAsync()
  - ClearAsync()
  - LengthAsync()
  - KeyAsync()
  - ContainsKeyAsync()
  
- synchronous via `ISyncLocalStorageService` (Synchronous methods are **only** available in Blazor WebAssembly):
  - SetItem()
  - GetItem()
  - RemoveItem()
  - Clear()
  - Length()
  - Key()
  - ContainsKey()

**Note:** Blazored.LocalStorage methods will handle the serialisation and de-serialisation of the data for you.
