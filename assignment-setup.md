# Setup instructions for visualization course assignments

These are intended for instructors and teaching staff only.

# Create Repo

1. Go to this year's staff GitHub organization. E.g., https://github.com/NEU-DS-4200-F20-Staff/

1. Create the new repository using a meaningful name, consistent with the other assignments for this class and with Canvas. E.g., `Assignment--Altair_Basic_Plots` with two dashes (--) between the type of assignment and the name of it, underscores separating words, and each word capitalized.

1. Clone the repository locally.

# Update the assignment

1. Copy in the files from last year and make any necessary updates, *including assignment links to Canvas.* Previous years:

    - https://github.com/NEU-DS-4200-F20-Staff ([Students](https://github.com/NEU-DS-4200-F20), [Classroom](https://classroom.github.com/classrooms/70919440-neu-ds-4200-f20))
    - https://github.com/NEU-CS-7250-S20-Staff ([Students](https://github.com/NEU-CS-7250-S20), [Classroom](https://classroom.github.com/classrooms/58606575-neu-cs-7250-s20))
    - https://github.com/NEU-DS-4200-S20-Staff ([Students](https://github.com/NEU-DS-4200-S20), [Classroom](https://classroom.github.com/classrooms/58606354-neu-ds-4200-s20))
    - https://github.com/Northeastern-DS-4200-F19-Staff ([Students](https://github.com/Northeastern-DS-4200-F19
    ), [Classroom](https://classroom.github.com/classrooms/55023881-northeastern-ds-4200-f19))

1. Do any necessary package/library updates. E.g., D3 versions, what's in `requirements.txt` and `requirements-noversions.txt`.

1. Make sure the code actually works and that nothing has broken.

1. Check all hyperlinks by hand, using the [W3 Link Checker](https://validator.w3.org/checklink), or using the GitHub actions validations.

1. Commit and push changes.

1. Make the repository Public.

# GitHub Pages (Web Development & D3 Only)

It is necessary if using GitHub Classroom to set up GitHub pages for the students, as they do not have admin permissions on their repository. To do this, we need to create and move everything to the `gh-pages` branch and delete the `master` branch.

1. Commit the files and push to the `master` branch on GitHub.

1. Then, run:

    ```
    git branch gh-pages
    git checkout gh-pages
    git branch -D master
    git push origin gh-pages
    ```

1. On GitHub, go to `Settings`->`Branches` and set the default branch to `gh-pages`.

1. Then, run:

    ```
    git push origin :master
    ```

# Clear the Git Commit History

In order to make things as straightforward for the students as possible, we want to give them only a single commit to start with.

* If you're using only the `master` branch run these commands:
    ```
    git checkout --orphan newBranch
    git add -A                      # Add all files and commit them
    git commit -m "Initial commit"
    git branch -D master            # Deletes the master branch
    git branch -m master            # Rename the current branch to master
    git push -f origin master       # Force push master branch to github
    git gc --aggressive --prune=all # remove the old files
    ```

* If you are using the `gh-pages` branch instead run:
    ```
    git checkout --orphan newBranch
    git add -A                      # Add all files and commit them
    git commit -m "Initial commit"
    git branch -D gh-pages          # Deletes the gh-pages branch
    git branch -m gh-pages          # Rename the current branch to gh-pages
    git push -f origin gh-pages     # Force push gh-pages branch to github
    git gc --aggressive --prune=all # remove the old files
    ```

# Template Repository

1. On GitHub, go to `Settings` and check the box for `Template repository` at the top. This makes GitHub Classroom copies much faster.

# GitHub Classroom

1. Create the assignment on GitHub Classroom.

1. Set the assignment title to be the same as on Canvas and consistent with the other assignments for this class.

1. Change the pre-set repository prefix so that it includes two dashes (--) between the type of assignment and the name of it. E.g., `in-class-programming--javascript` and `assignment--altair-and-jupyterlab-setup`.

1. Set the deadline (23:59 of the due day), make sure it matches the assignment on Canvas, and make sure it is Eastern time.

1. Set the template repository you created from above as the starter code.

1. Enable feedback pull requests.

1. Update assignment.

1. Enable the assignment invitation URL.

1. In the Canvas assignment, replace the old assignment invitation URL and template repo link URLs with the new ones.