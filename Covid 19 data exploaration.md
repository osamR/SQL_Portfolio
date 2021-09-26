# SQL_Portfolio
Covid19 Data Exploration

select *from PortfolioProject..CovidDeaths 



--  Death Percentage in Pakistan

select location,population,total_cases,total_deaths, new_cases ,(total_cases/total_deaths)*100 as  Death_Percentage 

from PortfolioProject..CovidDeaths where location like '%Pakistan%'
order by 1,2


-- Percentage of Higst infected per population in all over the world

Select location, population,Max(cast(Total_deaths as int)) as Higst_Deaths, MAX(total_cases) as Highst_infectionCount, MAX((total_cases/population))*100 as Highst_Death_Percentage
from PortfolioProject..CovidDeaths  
where continent is not Null

Group by population, location
order by Highst_Death_Percentage Desc


---- Highst death count  in world per population

Select location, population,Max(cast(Total_deaths as int)) as Highst_DeathCount
from PortfolioProject..CovidDeaths
where continent is not Null
Group by location, population
order by Highst_DeathCount Desc




-- Highst death count in continent 

Select continent, Date, MAX(cast(Total_deaths as int)) as Total_Death_count
From PortfolioProject..CovidDeaths

Where continent is not null 
Group by continent,date
order by Total_Death_count desc


--Global Numbers

Select  sum(new_cases) as total_cases, sum(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(new_cases)*100 as DeathPercentage
From PortfolioProject..CovidDeaths
where continent is not null 



--  looking at total population vs vaccination

select dea.continent, dea.location, vac.date,dea.population, vac.new_vaccinations,
SUM(convert(int,vac.new_vaccinations)) OVER (Partition by dea.location order by dea.location,vac.date ) as RollingPeopleVaccinated
from PortfolioProject..CovidDeaths dea
Join PortfolioProject..covidVaccinated$ vac
on dea.location=vac.location
  and  vac.date=dea.date
 
 where   dea.continent is not null
  order by 2,3




-- Using CTE to perform Calculation on Partition By in previous query

With PopvsVac (Continent, Location, Date, Population, New_Vaccinations, RollingPeopleVaccinated)
as
(
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated

From PortfolioProject..CovidDeaths dea
Join PortfolioProject..covidVaccinated$ vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null 
--order by 2,3
)
Select *, (RollingPeopleVaccinated/Population)*100 as Percentage_Vaccinated
From PopvsVac
