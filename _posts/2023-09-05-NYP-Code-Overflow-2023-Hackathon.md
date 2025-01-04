---
layout: post
title: "Healthy Foodie: An All-in-One Healthy Recipe Website"
subtitle: "NYP-Code-Overflow-2023-Hackathon"
author: "Junhui"
header-style: text
tags:
  - Web Development
  - Health
  - Hackathon
---

> This post showcases our group project **Healthy Foodie**, a website designed to combat rising obesity by offering accessible, healthy meal recipes.

---

## The Hackathon Experience

Participating in the hackathon was an incredible journey for our team, **Group 15**, as we tackled the challenge of promoting healthy eating with technology. Here's a snapshot of all the participants and judges at the event:

![Hackathon Participants and Judges](/img/in-post/NYP-Code-Overflow-2023/6O4A5846.JPG)

It was inspiring to be surrounded by so many talented individuals!

---

## Meet the Team

Here's the amazing team behind **Healthy Foodie**:
- **Misriya**: API Integration and Backend Developer
- **May**: UI/UX Designer and Frontend Developer
- **Junhui**: Database and System Architect
- **BingRui**: Project Manager and Quality Assurance

![Team Photo](/img/in-post/NYP-Code-Overflow-2023/6O4A5343.JPG)

Together, we brought our skills and creativity to the table to develop a functional and impactful website.

---

## The Problem: Rising Obesity Rates

### Global and Local Statistics
- **Singapore**: In 2020, **10.5% of adults were obese**, up from **8.6% in 2017**.
- **Globally**: **13% of adults are obese**, and **39% are overweight** (2016 statistics).

### Challenges of Healthy Eating
1. **Convenience**: Processed foods are faster and easier to prepare.
2. **Cost**: Healthy meals are often more expensive.
3. **Effort**: Preparing nutritious meals from scratch can be tedious.

### Health Consequences
Obesity is linked to numerous health issues, including:
- Diabetes
- Hypertension
- Heart Disease
- High Cholesterol

---

## Our Solution: Healthy Foodie

**Healthy Foodie** is a website designed to make healthy cooking accessible, affordable, and simple. By providing a rich database of recipes, users can find nutritious meals tailored to their preferences and dietary requirements.

### Key Features
1. **Recipe Search**:
   - Filter recipes by calories, protein, fats, allergies, and diets (e.g., vegan, keto).
2. **Personalized Recommendations**:
   - Input dietary needs and receive tailored recipe suggestions.
3. **Simple Interface**:
   - User-friendly design makes finding and preparing meals effortless.

### Technology Stack
- **Flask**: Backend development.
- **Spoonacular API**: Recipe and nutritional data.
- **SQLAlchemy**: Database management.
- **GitHub**: Collaboration and version control.

---

## Demonstrating Healthy Foodie

Presenting our project was one of the most exciting moments. As a team, we explained how **Healthy Foodie** works, emphasizing its ability to make healthy eating more accessible.

![Presenting on Stage](/img/in-post/NYP-Code-Overflow-2023/6O4A5699.JPG)

Our live demonstration showcased features like recipe filtering, personalized recommendations, and user-friendly design.

---

## My Role: Integrating the Spoonacular API

As the API integrator, I took charge of connecting our platform to the Spoonacular API, ensuring seamless access to dynamic recipe data. Here's a close-up of me presenting this key feature at the hackathon:

![Close-Up at the Podium](/img/in-post/NYP-Code-Overflow-2023/6O4A5710.JPG)

### API Integration Example

Below is a simplified Flask route for fetching and filtering recipes from the Spoonacular API:

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

### Search Feature Example

Users can search for recipes dynamically using the following JavaScript logic:

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

### Database Example

The database is managed with SQLAlchemy. Below is a model for storing recipe data:

```python
class Recipe(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(255), nullable=False)
```

This structure ensures we can save and retrieve recipes efficiently.

---

## Thank You!

We are immensely grateful to the organizers, judges, and fellow participants for making this hackathon a memorable and enriching experience.

Being part of such a vibrant community of innovators and creators was truly inspiring. Thank you for supporting **Healthy Foodie**!
```

You can copy the entire content directly and use it in your blog. Let me know if you need any further adjustments!