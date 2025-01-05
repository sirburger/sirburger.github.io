---
layout: post
title: "Healthy Foodie: An All-in-One Healthy Recipe Website"
subtitle: "Our Adventure at the NYP-Code-Overflow-2023 Hackathon"
author: "Junhui"
header-style: text
tags:
  - Python
  - Flask
  - SQL
  - Web Development
  - Health
  - Sustainability
  - Hackathon
---

> 🚀 **Healthy Foodie**: Tackling obesity, one recipe at a time!  
> Join us as we dive into our journey of creating a website that makes healthy eating simple and accessible.

---

## 🌟 The Hackathon Experience

Participating in the **NYP-Code-Overflow-2023 Hackathon** was an incredible adventure for our team, **Group 15**! We took on the challenge of promoting healthy eating through innovative technology.  

📸 Here's a snapshot of all the participants and judges who made the event unforgettable:  
![Hackathon Participants and Judges](/img/in-post/NYP-Code-Overflow-2023/6O4A5846.JPG)

✨ Surrounded by so many talented and passionate individuals, we were inspired to push our limits and create something impactful!

---

## 💪 Meet the Team

Behind every great project is an amazing team! Here’s **Group 15**, the minds behind **Healthy Foodie**:  
- **Misriya**: API Wizard 🪄 and Backend Guru  
- **May**: Design Maestro 🎨 and Frontend Pro  
- **Junhui**: Database Ninja 🥷 and System Architect  
- **BingRui**: Project Leader 👑 and QA Perfectionist  

📸 Check out our team photo:  
![Team Photo](/img/in-post/NYP-Code-Overflow-2023/6O4A5343.JPG)

Together, we combined creativity, technical skills, and sheer determination to bring **Healthy Foodie** to life.

---

## 🌍 The Problem: Rising Obesity Rates

### 📊 Alarming Statistics
- **Singapore**: In 2020, **10.5% of adults were obese**, up from **8.6% in 2017**.  
- **Globally**: **13% of adults are obese**, and **39% are overweight** (2016 data).

### 🚧 Challenges of Healthy Eating
1. **Convenience**: Processed foods are faster and easier.  
2. **Cost**: Healthy meals are often more expensive.  
3. **Effort**: Preparing healthy meals can be tedious.  

😢 These factors contribute to serious health issues, including diabetes, heart disease, and high cholesterol.

---

## 💡 Our Solution: Healthy Foodie

🌟 **Healthy Foodie** is a website designed to make healthy cooking accessible, affordable, and fun! By providing a rich database of recipes, users can find nutritious meals tailored to their preferences and dietary requirements.

### 🌈 Key Features
1. **✨ Recipe Search**:
   - Filter recipes by calories, protein, fats, allergies, and diets (e.g., vegan, keto).  
2. **🌟 Personalized Recommendations**:
   - Input your dietary needs and get custom recipes just for you!  
3. **💻 Simple Interface**:
   - Designed with users in mind, making healthy eating effortless.

### 🛠️ Technology Stack
- **Flask**: The backbone of our backend.  
- **Spoonacular API**: A treasure trove of recipes and nutritional data.  
- **SQLAlchemy**: Seamless database management.  
- **GitHub**: Collaboration made easy.

---

## 🎤 Demonstrating Healthy Foodie

One of the highlights of our hackathon experience was presenting **Healthy Foodie** to the judges and audience.  

📸 Here's a moment from our live demo:  
![Presenting on Stage](/img/in-post/NYP-Code-Overflow-2023/6O4A5699.JPG)  

Our presentation showcased key features like recipe filtering, personalized recommendations, and the user-friendly design we’re so proud of.

---

## 🛠️ My Role: Integrating the Spoonacular API

As the **API integrator**, my job was to connect our platform to the **Spoonacular API**. This allowed us to fetch dynamic recipe data and enable features like filtering by dietary preferences.  

📸 Here I am presenting this key feature:  
![Close-Up at the Podium](/img/in-post/NYP-Code-Overflow-2023/6O4A5710.JPG)

### 🔑 Key Contributions
1. **API Integration**:
   - Researched Spoonacular API to identify endpoints.
   - Built Flask routes to handle API calls and responses.
   - Enabled filtering by calories, protein, dietary restrictions, and more.  
2. **Data Parsing**:
   - Extracted data like recipe names, images, and nutritional values.
   - Ensured clean, structured results for a seamless user experience.  
3. **Error Handling**:
   - Managed API rate limits and handled invalid inputs gracefully.  
4. **Caching**:
   - Reduced repetitive API calls by caching popular recipes, improving performance.

### 🤔 Challenges
- **Rate Limits**: Balancing free-tier API restrictions with user demands.  
- **Complex Data**: Parsing and structuring large JSON responses.  
- **Custom Filtering**: Adapting API responses for user-specific dietary needs.  

### 🖥️ API Integration Example

```python
@app.route('/display_recommendations', methods=['GET'])
def display_recommendations():
    params = {
        'apiKey': SPOONACULAR_API_KEY,
        'maxCalories': request.args.get('maxcalories'),
        'minProtein': request.args.get('minprotein'),
        'maxFat': request.args.get('maxfat'),
        'excludeIngredients': request.args.get('allergies'),
        'diet': request.args.get('preferredDiet'),
        'number': 20,
        'sort': 'healthiness',
    }
    response = requests.get('https://api.spoonacular.com/recipes/complexSearch', params=params)
    if response.status_code == 200:
        recipes = response.json().get('results', [])
        return render_template('recommendations.html', datas=recipes)
    return "Failed to fetch recipes."
```

---

## 🔎 Search Feature Example

Our dynamic search feature allows users to easily find recipes:

```javascript
function getRecipe(query) {
    $.ajax({
        url: `https://api.spoonacular.com/recipes/search?apiKey=YOUR_API_KEY&query=${query}`,
        success: function(res) {
            const recipe = res.results[0];
            $("#output").html(`
                <h1>${recipe.title}</h1>
                <img src="${res.baseUri + recipe.image}" width="400" />
                <p>Ready in ${recipe.readyInMinutes} minutes</p>
            `);
        },
    });
}
```

---

## 📂 Database Example

We used SQLAlchemy to manage our database. Here’s a simple model:

```python
class Recipe(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(255), nullable=False)
```

This structure ensured efficient storage and retrieval of recipe data.

---

## ❤️ Thank You!

We are incredibly grateful to the organizers, judges, and fellow participants who made this hackathon such a fantastic experience.  

💖 I hope that i get better in web dev in the future
