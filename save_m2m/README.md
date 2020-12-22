# Saving a many to many field relation


Saving a form with many to many relation cause a
https://docs.djangoproject.com/en/3.1/topics/forms/modelforms/

when you have to save modal many to many and want mulitpl edata to be stored in the form 
there is a way
when you call `form.save()` on `form_valid` method
```python
def form_valid(self, form, *args, **kwargs):
        form.save()

if self.request.is_ajax():
    return JsonResponse({"result": "success", "content": "Item added"})
return super().form_valid(form)
```

it trigger `_save_m2m` method, write this method in your model form
this method is defined in `django.forms.BaseModelForm` class (tip: just ctrl+click on forms.ModelForm (i.e definition lookup))
feel free to copy
the default  `_save_m2m` method lookes like this

```python
def _save_m2m(self):
        """
        Save the many-to-many fields and generic relations for this form.
        """
        cleaned_data = self.cleaned_data
        exclude = self._meta.exclude
        fields = self._meta.fields
        opts = self.instance._meta
        # Note that for historical reasons we want to include also
        # private_fields here. (GenericRelation was previously a fake
        # m2m field).
        for f in chain(opts.many_to_many, opts.private_fields):
            if not hasattr(f, 'save_form_data'):
                continue
            if fields and f.name not in fields:
                continue
            if exclude and f.name in exclude:
                continue
            if f.name in cleaned_data:
                f.save_form_data(self.instance, cleaned_data[f.name])
                
```



write `_save_m2m` method in the ModelForm class in your `forms.py`

now i am going to explain my problem with this, the issue as well as my solution

# Example `model.py`

```python
class MasterImage(models.Model):
    image = models.ImageField(upload_to="inventory/%Y/%m/%d/")
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    class Meta:
        verbose_name = _("Master Image")
        verbose_name_plural = _("Master Images")
        db_table = 'master_image'

    def __str__(self):
        return self.image.name

class Item(models.Model):
    name = models.CharField(max_length=32)
    image = models.ManyToManyField('MasterImage', through="ItemImage")
    class Meta:
        verbose_name = _("Item")
        verbose_name_plural = _("Items")
        db_table = "item"

    def __str__(self):
        return self.name

    def get_absolute_url(self):
        return reverse("item_detail", kwargs={"pk": self.pk})

    def get_update_url(self):
        return reverse("inventory:item-update", kwargs={"pk": self.pk})


class ItemImage(models.Model):
    image = models.ForeignKey(MasterImage, on_delete=models.CASCADE)
    item = models.ForeignKey(Item, on_delete=models.CASCADE)

    class Meta:
        verbose_name = _("Item Image")
        verbose_name_plural = _("Item Images")
        db_table = 'item_image'

    def __str__(self):
        return self.image.image.name
```



```python
def _save_m2m(self):

```
