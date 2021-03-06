Quickstart
==========

1. Install ``django-popupcrud`` using pip: 

   ``pip install git+https://github.com/harikvpy/django-popupcrud.git``

   *Package has not yet been uploaded to PyPI, so until then install it directly
   from the repository.*

   Alternatively, you can clone this repository and install from the repo root
   folder via ``pip install -e .``.

2. Add ``popupcrud`` and its dependencies to INSTALLED_APPS in your project's
   settings.py::

       INSTALLED_APPS = [
           ...
           'bootstrap3',
           'pure_pagination',
           'popupcrud',
           ...
       ]

3. In your app's ``views.py``, create a ``ViewSet`` for each model for which you
   want to support CRUD operations.

   Models.py::
    

    from django.db import models

    class Author(models.Model):
        name = models.CharField("Name", max_length=128)
        penname = models.CharField("Pen Name", max_length=128)
        age = models.SmallIntegerField("Age", null=True, blank=True)

        class Meta:
            ordering = ('name',)
            verbose_name = "Author"
            verbose_name_plural = "Authors"

        def __str__(self):
            return self.name

   Views.py::

    from popupcrud.views import PopupCrudViewSet

    class AuthorViewSet(PopupCrudViewSet):
        model = Author
        fields = ('name', 'penname', 'age')
        list_display = ('name', 'penname', 'age')
        list_url = reverse_lazy("library:authors")
        new_url = reverse_lazy("library:new-author")

        def get_edit_url(self, obj):
            return reverse_lazy("library:edit-author", kwargs={'pk': obj.pk})

        def get_delete_url(self, obj):
            return reverse_lazy("library:delete-author", kwargs={'pk': obj.pk})

4. Wire up the individual CRUD views generated by the viewset to the URL 
   namespace::

    urlpatterns= [
        url(r'^authors/$', views.AuthorCrudViewset.list(), name='authors'),
        url(r'^authors/new/$', views.AuthorCrudViewset.create(), name='new-author'),
        url(r'^authors(?P<pk>\d+)/edit/$', views.AuthorCrudViewset.update(), name='edit-author'),
        url(r'^authors(?P<pk>\d+)/delete/$', views.AuthorCrudViewset.delete(), name='delete-author'),
        ]

5. Thats it! Your modern HTML popup based CRUD for your table is up and running.
   PopupCrudViewSet has many options to customize the fields displayed in list
   view, form used for create/update operations, permission control and more.
