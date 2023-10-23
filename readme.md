htmx gives you access to AJAX, CSS Transitions, WebSockets and Server Sent Events directly in HTML, using attributes, so you can build modern user interfaces with the simplicity and power of hypertext

htmx is small (~14k min.gz’d), dependency-free, extendable, IE11 compatible & has reduced code base sizes by 67% when compared with react

Essential HTMX attributes
hx-target:
Read the documentation about hx-target via https://htmx.org/attributes/hx-target/


hx-trigger
Read the documentation about hx-trigger via https://htmx.org/attributes/hx-trigger/

hx-post
Read the documentation about hx-post via https://htmx.org/attributes/hx-post/


Case utilisation

urls.py
```
from django.urls import path
from films import views
from django.contrib.auth.views import LogoutView

urlpatterns = [
    path("register/", views.RegisterView.as_view(), name="register"),
]

htmx_urlpatterns = [
    path('check_username/', views.check_username, name='check-username'),
]

urlpatterns += htmx_urlpatterns
```
views.py
```
def check_username(request):
    username = request.POST.get('username')
    if get_user_model().objects.filter(username=username).exists():
        return HttpResponse("<div id='username-error' class='error'>This username already exists</div>")
    else:
        return HttpResponse("<div id='username-error' class='success'>This username is available</div>")
```

register.html
```
<form method="POST" action="{% url 'register' %}" autocomplete='off'>
    {% csrf_token %}

    <div class="form-group mb-3">
        <label>{{ form.username.label_tag }}</label>
        {{ form.username.errors }}
        {% render_field form.username hx-post="/check-username/" hx-target="#username-err" hx-trigger="keyup delay:2s" class="form-control" %}
        <div class="mt-2" id="username-err"></div>
    </div>
    <div>

        <label>{{ form.password1.label_tag }}</label>
        {{ form.password1.errors }}
        {% render_field form.password1 class="form-control" %}
    </div>
    <div>
        <label>{{ form.password2.label_tag }}</label>
        {{ form.password2.errors }}
        {% render_field form.password2 class="form-control" %}
    </div>
    
    <button type="submit" class="btn btn-success mt-2">Register</button>
</form>
```