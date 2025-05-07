**Stop Using `NoContent()` in ABP Modals — Here's What to Do Instead**
---

## 🧩 Use Case: Profile Deletion

Imagine a scenario where a user wants to delete their account. You want:

* A confirmation modal
* Deletion to happen server-side
* A clean redirect to the goodbye page

---

## 💡 Razor Page Modal

```cshtml
@page
@model DeleteAccountModalModel
@{
    Layout = null;
}
<form method="post">
    <abp-modal>
        <abp-modal-body>
            <p>Are you sure you want to delete your account?</p>
        </abp-modal-body>
        <abp-modal-footer buttons="Save,Cancel"></abp-modal-footer>
    </abp-modal>
</form>
```

---

## 🧠 OnPostAsync Logic

```csharp
public async Task<IActionResult> OnPostAsync()
{
    await _userAppService.DeleteCurrentAsync();
    return new JsonResult(new { redirect = "/Goodbye" });
}
```

---

## 📟 JavaScript with ModalManager

```js
const deleteAccountModal = new abp.ModalManager({
    viewUrl: '/Account/DeleteAccountModal'
});

$('#delete-account-btn').click(() => deleteAccountModal.open());

deleteAccountModal.onResult((result) => {
    if (result?.redirect) {
        window.location.href = result.redirect;
    }
});
```

---

## ✅ Result

* ABP posts the form automatically
* Modal closes
* `onResult()` triggers with `{ redirect: '/Goodbye' }`
* The user is cleanly redirected
