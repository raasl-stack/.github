# RAASL STACK UNIPI

# ⚠️ Visibility Information ⚠️ 

This organization is set to `public`, meaning anyone can find it by searching on GitHub or accessing the page via its URL; however, the members list is private.

To avoid issues, when creating a new repository, set it to `private` so that the code is only visible to the organization's members.

# 📁 Guide to branches

To create a new branch for a repository follow this steps:
1. Clone the repo or navigate inside the repo main folder
2. Move to the desired branch with:
```
git checkout -b devel
```
This command will create a new local branch and move to this branch

3. Add your files to the new branch:
```
git add .
```
4. Commit on the branch:
```
git commit -m "Commit on branch 'devel'
```
5. Publish the new branch on github:
 ```
 git push -u origin devel
 ```


# 📁 Guide to Using Git Submodules in a ROS Workspace

## 🔹 What Are Git Submodules?

**Git submodules** allow you to include one Git repository inside another. In our case, we use submodules to include ROS packages (e.g., messages, planners, navigation stacks...) into a **generic workspace**, while still keeping them as independent projects.

🔧 Benefits:
- Code reuse across multiple projects
- Logical separation of components
- Independent version control for each module

---

## 🔹 How to Add Submodules to a New Workspace

If you're creating a **new workspace** and want to "link" existing submodules, follow these steps:

```bash
mkdir -p my_ws/src
cd my_ws/src

# Add the submodules
git submodule add https://github.com/your-user/robot_msgs_a.git
git submodule add https://github.com/your-user/robot_msgs_b.git

cd ..
# Initialize the workspace
catkin_make
```

⚠️ After adding the submodules, **don’t forget to commit** the `.gitmodules` file and the submodule folders (they act as pointers to specific commits):

```bash
git add .gitmodules src/*
git commit -m "Added robot_msgs submodules"
git push
```

---

## 🔹 How to Work with Submodules (for collaborators cloning the repo)

When a collaborator clones the main workspace repository (`robot_ws`):

```bash
git clone --recurse-submodules https://github.com/your-user/robot_ws.git
cd robot_ws
catkin_make
```

If the repo has already been cloned without `--recurse-submodules`, initialize the submodules later with:

```bash
git submodule update --init --recursive
```

---

## 🔄 How to Update Submodules

If a submodule has been **updated by someone else** (e.g., new code pushed to `robot_msgs_a`), you can update it like this:

```bash
cd src/robot_msgs_a
git pull origin main  # or master
cd ../..

# Register the update in the main repository
git add src/robot_msgs_a
git commit -m "Updated robot_msgs_a submodule"
git push
```

🧠 Each submodule points to a specific commit. Even if the submodule's repo has new changes, **the main workspace won’t see them until you update and commit the new pointer**.

---

## 🔁 Useful Git Submodule Commands

| Task                            | Command                                                       |
|---------------------------------|---------------------------------------------------------------|
| Clone everything with submodules | `git clone --recurse-submodules <repo>`                      |
| Initialize submodules            | `git submodule update --init --recursive`                    |
| Update all submodules            | `git submodule update --remote --merge`                      |
| Add a new submodule              | `git submodule add <url> path/to/target`                     |
| Remove a submodule               | [Official guide](https://git-scm.com/book/en/v2/Git-Tools-Submodules#_removing_submodules) |
| Check submodule status           | `git submodule status`                                       |
