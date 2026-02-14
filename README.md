📘 Django CRUD – Easy Documentation

A beginner-friendly guide to understanding CRUD operations in Django.

📌 CRUD কী?

CRUD মানে ৪টা কাজ:

Create → নতুন ডাটা যোগ করা

Read → ডাটা দেখানো

Update (Edit) → পুরনো ডাটা পরিবর্তন করা

Delete → ডাটা মুছে ফেলা

🟢 1️⃣ CREATE (নতুন ডাটা তৈরি)
🔹 Step 1: Form বানাতে হবে
<form method="POST">
  {% csrf_token %}

  <input type="text" name="name">
  <input type="email" name="email">

  <input type="submit" value="Submit">
</form>

🔹 Step 2: views.py তে POST নিয়ে save করতে হবে
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

🟢 2️⃣ READ (ডাটা দেখানো)
🔹 views.py
def doctor_list(request):

    doctors = Doctormodel.objects.all()

    return render(request, 'list.html', {
        'doctors': doctors
    })

🔹 Template এ দেখানো
{% for d in doctors %}
  <p>{{ d.name }} - {{ d.email }}</p>
{% endfor %}

🟢 3️⃣ UPDATE (EDIT) – সবচেয়ে গুরুত্বপূর্ণ অংশ
📌 Edit করার নিয়ম

id দিয়ে object বের করতে হবে

template এ পাঠাতে হবে

input এর value তে বসাতে হবে

POST এ modify করে save() করতে হবে

🔹 views.py
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

🔹 edit.html
⭐ Value বসানো
<input type="text" name="name" value="{{doctor.name}}">
<input type="email" name="email" value="{{doctor.email}}">

👉 নিয়ম:
value="{{ context_variable.model_field }}"

📌 যদি Select Option থাকে
<select name="category">

  <option value="A"
  {% if doctor.category == 'A' %}
  selected
  {% endif %}
  >
  A
  </option>

</select>


👉 {% if %} দিয়ে selected করতে হবে।

📌 যদি ForeignKey থাকে
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

🟢 4️⃣ DELETE
🔹 views.py
def deleteDoctor(request, id):

    Doctormodel.objects.get(id=id).delete()

    return redirect('doctor_list')

📊 CRUD এক লাইনে মনে রাখার নিয়ম
কাজ	কী করবো
Create	create()
Read	all()
Update	get() → modify → save()
Delete	get() → delete()
🎯 Edit এর Golden Rule

✔ id দিয়ে get()

✔ value="{{data.field}}"

✔ select হলে {% if %} দিয়ে selected

✔ শেষে save()

✔ {% csrf_token %} দিতে হবে

🧠 Complete Flow
User → Form → POST → views.py → save() → redirect → list page
