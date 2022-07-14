Select * from PortfolioProject..CovidDeaths
order by 3,4


--Select * from PortfolioProject..CovidVaccinations
--order by 3,4


--total cases vs total deaths
Select location, date, total_cases,total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
from PortfolioProject..CovidDeaths
order by 1,2

--total cases vs population
-- shows what % population got covid

Select location, date, total_cases,Population, (total_cases/population)*100 as PercentofPopInfected
from PortfolioProject..CovidDeaths
Where location like '%state%'
order by 1,2

-- countries with highest infection rate compared to population

Select location, MAX(total_cases) as HighestInfectionCount,Population, MAX((total_cases/population))*100 as PercentofPopInfected
from PortfolioProject..CovidDeaths
group by Location, Population
order by PercentofPopInfected desc



-- Showing Countries With Highest Death Count per Population

Select location, MAX(cast(total_deaths as bigint)) as TotalDeathCount
from PortfolioProject..CovidDeaths
Where continent is not null
group by Location
order by TotalDeathCount desc


-- Showing Continent With Highest Death Count per Population


Select location, MAX(cast(total_deaths as bigint)) as TotalDeathCount
from PortfolioProject..CovidDeaths
Where continent is null
and not location = 'lower middle income'
and not location = 'high income'
and not location = 'upper middle income'
and not location = 'low income'
and not location = 'international'
group by location
order by TotalDeathCount desc

-- Global Nummbers

Select date, sum(new_cases) as totalCases, sum(cast(new_deaths as int)) as totalDeaths, sum(cast(new_deaths as int))/sum(new_cases)*100 as deathPercent 
from PortfolioProject..CovidDeaths
Where continent is not null
and not location = 'lower middle income'
and not location = 'high income'
and not location = 'upper middle income'
and not location = 'low income'
and not location = 'international'
group by date
order by 1,2

Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(bigint,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
--, (RollingPeopleVaccinated/population)*100
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null 
order by 2,3

With PopvsVac (Continent, Location, Date, Population, New_Vaccinations, RollingPeopleVaccinated)
as
(
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(bigint,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null 
)
Select *, (RollingPeopleVaccinated/Population)*100
From PopvsVac


-- Creating View to store data for later visualizations

Create View PercentPopulationVaccinated as
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(bigint,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null 
