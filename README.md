🏥 Django CRUD Application
<p align="center"> <b>A Beginner Friendly Django CRUD Project</b><br> Create • Read • Update • Delete Operations </p>
📌 Project Overview

This project demonstrates how to implement CRUD (Create, Read, Update, Delete) operations in Django using:

🧱 Models

🖥 Views

🎨 Templates

🔗 URL Routing

📦 Form Handling

📚 What is CRUD?
Operation	Meaning
🟢 Create	Add new data
🔵 Read	Show data
🟡 Update	Edit existing data
🔴 Delete	Remove data
🟢 1️⃣ CREATE – Add New Data
📝 HTML Form
<form method="POST">
  {% csrf_token %}

  <input type="text" name="name">
  <input type="email" name="email">

  <input type="submit" value="Submit">
</form>

⚙ views.py
def createDoctor(request):

    if request.method == "POST":

        name = request.POST.get('name')
        email = request.POST.get('email')

        Doctormodel.objects.create(
            name=name,
            email=email
        )

        return redirect('doctor_list')

    return render(request, 'create.html')

🔵 2️⃣ READ – Show Data
⚙ views.py
def doctor_list(request):

    doctors = Doctormodel.objects.all()

    return render(request, 'list.html', {
        'doctors': doctors
    })

🖥 Template
{% for d in doctors %}
  <p>{{ d.name }} - {{ d.email }}</p>
{% endfor %}

🟡 3️⃣ UPDATE – Edit Data
🔄 Edit Process

Get object using id

Send object to template

Set input value

Modify & save()

⚙ views.py
def editDoctor(request, id):

    doctor = Doctormodel.objects.get(id=id)

    if request.method == "POST":

        doctor.name = request.POST.get('name')
        doctor.email = request.POST.get('email')

        doctor.save()

        return redirect('doctor_list')

    return render(request, 'edit.html', {
        'doctor': doctor
    })

🖥 edit.html
✅ Value Binding
<input type="text" name="name" value="{{doctor.name}}">
<input type="email" name="email" value="{{doctor.email}}">

✅ Select Option Example
<select name="category">

  <option value="A"
  {% if doctor.category == 'A' %}
  selected
  {% endif %}
  >
  A
  </option>

</select>

✅ ForeignKey Example
<select name="department">

{% for i in department_data %}

<option value="{{i.id}}"
{% if doctor.department.id == i.id %}
selected
{% endif %}
>
{{i.name}}
</option>

{% endfor %}
</select>

🔴 4️⃣ DELETE – Remove Data
⚙ views.py
def deleteDoctor(request, id):

    Doctormodel.objects.get(id=id).delete()

    return redirect('doctor_list')

📊 CRUD Logic Summary
Action	Method
Create	objects.create()
Read	objects.all()
Update	get() → modify → save()
Delete	get() → delete()
🎯 Edit Golden Rules

✔ Use get(id)

✔ Use value="{{object.field}}"

✔ Use {% if %} for selected option

✔ Always include {% csrf_token %}

✔ Call save() after modification

🔄 Application Flow
User → Fill Form → POST → views.py → save() → redirect → list page

Edit → get(id) → show data → update → save()

Delete → get(id) → delete() → redirect

🚀 Getting Started
git clone <your-repo-link>
cd your-project
python manage.py runserver

⭐ Support

If you found this project helpful, please ⭐ star the repository.
